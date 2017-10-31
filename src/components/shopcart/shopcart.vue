<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <span class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></span>
          </div>
          <div class="num" v-show="totalCount">
            {{ totalCount }}
          </div>
        </div>
        <div class="price" :class="{'highlight':totalCount>0}">￥{{ totalPrice }}</div>
        <div class="desc">另需配送费￥{{ deliveryPrice }}元</div>
      </div>
      <div class="content-right" :class="{'highlight':payDesc===-1}">
        <div class="pay" v-if="payDesc===0">￥{{ minPrice }}元起送</div>
        <div class="pay highlight" v-if="payDesc===-1">去结算</div>
        <div class="pay" v-if="payDesc>0">还差￥{{ payDesc }}元起送</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'shopcart',
  props: {
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    },
    selectFoods: {
      type: Array,
      default () {
        return []
      }
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalCount === 0) {
        return 0
      } else {
        if (this.totalPrice < this.minPrice) {
          let money = this.minPrice - this.totalPrice
          console.log(money)
          return money
        } else {
          return -1
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" ref="stylesheet/stylus" scoped>
  .shopcart
    position: fixed
    left: 0
    bottom: 0
    z-index: 50
    width: 100%
    height: 48px
    .content
      display: flex
      background: #141d27
      font-size: 0
      .content-left
        flex: 1
        .logo-wrapper
          display: inline-block
          position: relative
          top: -10px
          margin: 0 12px 
          padding: 6px
          width: 56px
          height: 56px
          box-sizing: border-box
          vertical-align: top
          border-radius: 50%
          background: #141d27
          text-align: center
          .logo
            width: 100%
            height: 100%
            border-radius: 50%
            background: #2b343c
            &.highlight
              background: rgb(0, 160, 220)
            .icon-shopping_cart
              font-size: 24px
              line-height: 44px
              color: #80858a
              &.highlight
                color: #fff
          .num
            position: absolute
            top: 0 
            right: 0
            width: 24px
            height: 16px
            line-height: 16px
            text-align: center
            background: red
            border-radius: 16px
            color: #fff
            font-size: 9px
            font-weight: 700
            box-shoadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        .price
          display: inline-block
          vertical-align: top
          line-height: 24px
          margin-top: 12px
          box-sizing: border-box
          padding-right: 12px
          border-right: 1px solid rgba(255, 255, 255, 0.1)
          font-size: 16px
          font-weight: 700
          color: rgba(255, 255, 255, 0.4)
          &.highlight
            color: #fff
        .desc
          display: inline-block
          vertical-align: top
          margin: 12px 0 0 12px
          line-height: 24px
          color: rgba(255, 255, 255, 0.4)
          font-size: 10px
      .content-right
        flex: 0 0 105px
        width: 105px
        background: #2b333b
        .pay
          margin: 0 8px
          font-size: 12px
          height: 48px
          line-height: 48px
          text-align: center
          color: rgba(255, 255, 255, 0.4)
          font-weight: 700
          &.highlight
            color: #fff
        &.highlight
          background: #00b43c
</style>
