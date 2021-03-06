<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{ seller.name }}</span>
        </div>
        <div class="description">
          {{ seller.description }}/{{ seller.deliveryTime }}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <iconType :size="1" :type="seller.supports[0].type" :right="4" :font="12"></iconType>
          <span class="text">{{ seller.supports[0].description }}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="detailShow=true">
        <span class="count">{{ seller.supports.length }}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span><span class="bulletin-text">{{ seller.bulletin }}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" >
    </div>
    <transition name="fade">
    <div class="detail" v-show="detailShow">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{ seller.name }}</h1>
          <div class="star-wrapper">
            <v-star :score="seller.score" :size="48"></v-star>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul class="supports" v-if="seller.supports">
            <li class="support-item" v-for="(item, index) in seller.supports">
              <iconType :size="1" :type="item.type" :right="6" :font="16"></iconType>
              <span class="text">{{ item.description }}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="content">{{ seller.bulletin }}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="detailShow=false">
        <i class="icon-close"></i>
      </div>
    </div>
    </transition>
  </div>
</template>

<script>
import star from '../star/star'
import iconType from '../icon/iconType'

export default {
  name: 'header',
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      detailShow: false
    }
  },
  components: {
    'v-star': star,
    iconType
  }
}
</script>


<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin"

  .header
    color: #fff
    overflow: hidden
    position: relative
    background: rgba(7, 17, 27, 0.4)
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      font-size: 0
      .avatar
        display: inline-block
        vertical-align: top
        img
          border-radius: 2px
          height: 64px
          width: 64px
      .content
        display: inline-block
        font-size: 14px
        margin-left: 16px
        .title
          margin: 2px 0 8px 0
          .brand
            display: inline-block
            vertical-align: top
            width: 30px
            height: 18px
            bg-image('brand')
            background-size: 30px 18px
            background-repeat: no-repeat
          .name
            margin-left: 6px
            font-size: 16px
            line-height: 18px
            font-weight: bold
        .description
          margin-bottom: 10px
          font-size: 12px
          line-height: 12px
        .support
          .text
            line-height: 12px
            font-size: 10px
            vertical-align: top
      .support-count
        position: absolute
        right: 12px
        bottom: 18px
        padding: 0 8px
        height: 24px
        line-height: 24px
        border-radius: 14px
        background: rgba(0, 0, 0, 0.2)
        text-align: center
        .count
          vertical-align: top
          font-size: 10px
        .icon-keyboard_arrow_right
          line-height: 24px
          margin-left: 2px
          font-size: 10px
    .bulletin-wrapper
      position: relative
      height: 28px
      line-height: 28px
      padding: 0 22px 0 12px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      background: rgba(7, 17, 27,0.2)
      .bulletin-title
        vertical-align: top
        display: inline-block
        width: 22px
        height: 12px
        margin-top: 8px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
      .bulletin-text
        vertical-align: top
        font-size: 10px
        margin: 0 4px
      .icon-keyboard_arrow_right
        position: absolute
        font-size: 10px
        right: 12px
        top: 8px
    .background
      position: absolute
      top: 0
      left: 0
      z-index: -1
      height: 100%
      width: 100%
      filter: blur(10px)
      img
        width: 100%
        height: 100%
    .detail
      position: fixed
      z-index: 100
      width: 100%
      height: 100%
      overflow: auto
      top: 0
      left: 0
      transition: all 0.5s
      background: rgba(7, 17, 27, 0.8)
      backdrop-filter: blur(10px)
      &.fade-enter-active, &.fade-leave-active
        opacity: 1
        background: rgba(7, 17, 27, 0.8)
      &.fade-enter, &.fade-leave-to
        opacity: 0
        background: rgba(7, 17, 27, 0)
      .detail-wrapper
        min-height: 100%
        width: 100%
        .detail-main
          margin-top: 64px
          padding-bottom: 64px
          .name
            line-height: 16px
            text-align: center
            font-size: 16px
            font-weight: 700
          .star-wrapper
            margin-top: 18px
            padding: 2px 0
            text-align: center
          .title
            display: flex
            width: 80%
            margin: 28px auto 24px auto
            .line
              flex: 1
              position: relative
              top: -6px
              border-bottom : 1px solid rgba(255, 255, 255, 0.2)
            .text
              padding: 0 12px
              font-size: 14px
              font-weight: 700
          .supports
            width: 80%
            margin: 0 auto
            .support-item
              padding: 0 12px
              margin-bottom: 12px
              font-size: 0
              &:last-child
                margin-bottom: 0
              .text
                display: inline-block
                font-size: 12px
                line-height: 16px 
          .bulletin
            width: 80%
            margin: 0 auto
            .content
              padding: 0 12px
              font-size: 12px
              line-height: 24px
      .detail-close
        position: relative
        margin: -64px auto 0 auto
        line-height: 32px
        width: 32px
        height: 32px
        clear: both
        font-size: 32px
</style>
