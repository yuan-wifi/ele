<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item in goods" :key="item.id" class="menu-item">
          <span class="text border-1px">
            <iconType :size="3" :type="item.type" :right="2" :font="12"></iconType>{{ item.name }}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h1 class="title">{{ item.name }}</h1>
          <ul>
            <li v-for="food in item.foods" :key="food.id" class="food-item">
              <div class="icon">
                <img :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{ food.name }}</h2>
                <p class="desc">{{ food.description }}</p>
                <div class="extra">
                  <span>月售{{ food.sellCount }}份</span>
                  <span>好评率{{ food.rating }}%</span>
                </div>
                <div class="price">
                  ￥<span class="now-price">{{ food.price }}</span>
                  <span v-show="food.oldPrice" class="old-price">￥{{ food.oldPrice }}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import iconType from '../icon/iconType'

export default {
  name: 'goods',
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: []
    }
  },
  created () {
    // 调用商品信息
    this.$axios.get('/api/goods').then((response) => {
      this.goods = response.data.data
    }, (error) => {
      console.log(error)
    })
  },
  components: {
    iconType
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" ref="stylesheet/stylus" scoped>
  @import "../../common/stylus/mixin"

  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    .menu-wrapper
      flex: 0 0 80px
      background: #f3f5f7
      width: 80px
      .menu-item
        display: table
        height: 54px
        width: 56px
        line-height: 14px
        padding: 0 12px
        .text
          display: table-cell
          width: 56px
          vertical-align: middle
          font-size: 12px
          border-1px(rgba(7, 17, 27, 0.1))
    .foods-wrapper
      flex: 1
      .title
        line-height: 26px
        font-size: 12px
        color: rgba(147, 153, 159, 1)
        background: #f3f5f7
        height: 26px
        &:before
          line-height: 26px
          height: 26px
          width: 2px
          diplay: block
          position: absolute
          left: 0
          bottom: 0
          border-top: 1px solid #d9dde1
          content: ''
      .food-item
        position: relative
        padding: 18px 18px 18px 18px 
        .icon
          display: inline-block
          vertical-align: top
          img
            border-radius: 2px
            height: 58px
            width: 58px
        .content
          display: inline-block
          vertical-align: top
          margin-left: 10px
          font-size: 0
          .name
            font-size: 14px
            color: rgba(7, 17, 27 ,1)
            line-height: 14px
            font-weight: bold
            margin: 2px auto 8px auto
          .desc, .extra
            display: inline-block
            line-height: 10px
            font-size: 10px
            color: rgba(147, 153, 159, 1)
            margin-bottom: 8px
          .price
            font-size: 10px
            font-weight: normal
            color: red
            .now-price
              line-height: 24px
              font-size: 14px
              font-weight: 700
            .old-price
              line-height: 24px
              font-size: 10px
              color: rgba(147, 153, 159, 1)
              font-weight: 700
</style>
