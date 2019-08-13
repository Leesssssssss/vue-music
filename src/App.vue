<template>
  <div id="app">
    <m-header></m-header>
    <tab></tab>

    <!-- keep-alive让router-view中的dom保存在内存中，不需要重复加载 -->
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
import MHeader from 'components/m-header/m-header'
import Tab from 'components/tab/tab'

export default {
  components: {
    MHeader,
    Tab
  }
}
</script>

<style scoped lang="stylus" rel="stylessheet/stylus"></style>
