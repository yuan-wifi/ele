<template>
  <div id="app">
    <div class="header">
      <v-header :seller="seller"></v-header>
    </div>
    <div class="tab border-1px">
      <div class="tab-item">
        <!-- 使用 router-link 组件来导航. -->
        <!-- 通过传入 `to` 属性指定链接. -->
        <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
</template>

<script>
import header from './components/header/header'

export default {
  name: 'app',
  data () {
    return {
      seller: {},
      goods: {},
      ratings: {}
    }
  },
  components: {
    'v-header': header
  },
  created () {
    // 获取商家信息
    this.$axios.get('/api/seller').then((response) => {
      this.seller = response.data.data
    }, (error) => {
      console.log(error)
    })

    // 获取评论信息
    this.$axios.get('/api/ratings').then((response) => {
      this.ratings = response.data
    }, (error) => {
      console.log(error)
    })
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import './common/stylus/mixin.styl'

  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height: 40px
      // border-bottom: 1px solid rgba(7, 17,27, 0.1)
      border-1px(rgba(7, 17,27, 0.1))
      .tab-item
        flex: 1
        text-align: center
        & > a
          display: block
          font-size: 14px
          color: rgb(77, 85, 93)
          &.active 
            color: rgb(240, 20, 20)
</style>
