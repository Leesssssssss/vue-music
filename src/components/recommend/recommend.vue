<template>
  <div>
    <div v-if="recommends.length" class="slider-wrapper">
      <slider>
        <div v-for="item in recommends" :key="item.id">
          <a :href="item.linkUrl">
            <img :src="item.picUrl" alt="">
          </a>
        </div>
      </slider>
    </div>
  </div>
</template>

<script>
import Slider from 'base/slider/slider'
import {getRecommend} from 'api/recommend'
import {ERR_OK} from 'api/config'

export default {
  data () {
    return {
      recommends: []
    }
  },
  components: {
    Slider
  },
  created () {
    this._getRecommend()
  },
  methods: {
    _getRecommend () {
      getRecommend().then((res) => {
        console.log(res.data.slider.length)
        if (res.code === ERR_OK) {
          this.recommends = res.data.slider
        }
      })
    }
  }
}
</script>

<style scoped lang='stylus' rel='stylessheet/stylus'>
.slider-wrapper
  position: relative;
</style>
