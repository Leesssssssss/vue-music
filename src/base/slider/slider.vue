<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot></slot>
    </div>
    <div class="dots">
      <span class="dot" v-for="(item, index) in dots" :class="{active: currentPageIndex === index}"></span>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import { addClass } from 'common/js/dom'

export default {
  data () {
    return {
      dots: [],
      currentPageIndex: 0
    }
  },
  props: {
    loop: {
      type: Boolean,
      default: true
    },
    autoPlay: {
      type: Boolean,
      default: true
    },
    interval: {
      type: Number,
      default: 4000
    }
  },
  mounted () {
    setTimeout(() => {
      this._setSliderWidth()
      this._initdots()
      this._initSlider()
      if (this.autoPlay) {
        this._play()
      }
    }, 20)

    // 监听窗口宽度变化去改变轮播图宽度
    window.addEventListener("resize", () => {
      if (!this.slider) {
        return
      }
      this._setSliderWidth(true)
      this.slider.refresh()
    });
  },
  methods: {
    // 设置轮播图宽度
    _setSliderWidth (isResize) {
      this.children = this.$refs.sliderGroup.children

      let width = 0;
      let sliderWidth = this.$refs.slider.clientWidth
      for (let i = 0; i < this.children.length; i++) {
        let child = this.children[i]
        addClass(child, "slider-item")

        child.style.width = sliderWidth + "px"
        width += sliderWidth
      }
      if (this.loop && !isResize) {
        width += 2 * sliderWidth
      }
      this.$refs.sliderGroup.style.width = width + "px"
    },

    // 初始化轮播图下方dots
    _initdots () {
      this.dots = new Array(this.children.length)
    },

    // 初始化轮播图
    _initSlider () {
      this.slider = new BScroll(this.$refs.slider, {
        scrollX: true,
        scrollY: false,
        momentum: false,
        // snap: true,
        // snapLoop: true,
        // snapThreshold: 0.3,
        // snapSpeed: 400
        snap: {
          // 新版本将snap的属性都当成一个对象来书写
          loop: this.loop, // 循环
          threshold: 0.3,
          speed: 400 // 轮播间隔
        }
      })

      this.slider.on("scrollEnd", () => {
        let pageIndex = this.slider.getCurrentPage().pageX   //轮播到下一张，获取当前的index
        // if (this.loop) {             //旧版本设置方式，新版本不需要
        //   pageIndex -= 1
        // }
        this.currentPageIndex = pageIndex

        if (this.autoPlay) {
          clearTimeout(this.timer)
          this._play()
        }
      })
    },

    // 设置轮播图自动播放
    _play () {
      // let pageIndex = this.currentPageIndex;         //旧版本需要计算增加的两张图片带来的影响
      // if (this.loop) {
      //   pageIndex += 1
      // }
      this.timer = setTimeout(() => {
        // this.slider.goToPage(pageIndex, 0, 400);
        this.slider.next()
      }, this.interval)
    }
  },

  // 销毁定时器有利于内存的释放
  destroyed () {
    clearTimeout(this.timer)
  }
}
</script>

<style scoped lang='stylus' rel='stylessheet/stylus'>
@import '~common/stylus/variable'
.slider
  min-height 1px
  .slider-group
    position relative
    overflow hidden
    white-space nowrap
    .slider-item
      float left
      box-sizing border-box
      overflow hidden
      text-align center
      a
        display block
        width 100%
        overflow hidden
        text-decoration none
      img
        display block
        width 100%
  .dots
    position absolute
    right 0
    left 0
    bottom 12px
    text-align center
    font-size 0
    .dot
      display inline-block
      margin 0 4px
      width 8px
      height 8px
      border-radius 50%
      background $color-text-l
      &.active
        width 20px
        border-radius 5px
        background $color-text-ll
</style>
