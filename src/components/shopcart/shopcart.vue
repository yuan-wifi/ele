<template>
  <div class="shopcart">
    <div class="content" @click="toggleList">
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
    <div class="ball-container">
      <transition name="drop" v-for="ball in balls" :key="ball.id" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
        <div class="ball" v-show="ball.show">
          <div class="inner inner-hook">
          </div>
        </div>
      </transition>
    </div>
    <div class="shopcart-list" v-show="listShow">
      <div class="list-header">
        <h1 class="title"></h1>
        <span class="empty"></span>
      </div>
      <div class="list-content">
        <li class="food" v-for="food in selectFoods">
          <span class="name">{{ food.name }}</span>
          <div class="price">
            <span>￥{{ food.price*food.count }}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol :food="food"></cartcontrol>
          </div>
        </li>
      </div>
    </div>
  </div>
</template>

<script>
import cartcontrol from '../cartcontrol/cartcontrol'

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
  data () {
    return {
      balls: [
        {'show': false},
        {'show': false},
        {'show': false},
        {'show': false},
        {'show': false}
      ],
      dropBalls: [],
      fold: false
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
          return money
        } else {
          return -1
        }
      }
    },
    listShow () {
      if (!this.totalCount) {
        this.fold = true
        return false
      }
      let show = !this.fold
      return show
    }
  },
  methods: {
    drop (el) {
      // 触发一次事件将会将所有小球进行遍历
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i]
        if (!ball.show) {
          ball.show = true
          ball.el = el
          this.dropBalls.push(ball)
          return
        }
      }
    },
    beforeEnter (el) {
      let count = this.balls.length
      while (count--) {
        let ball = this.balls[count]
        if (ball.show) {
          let rect = ball.el.getBoundingClientRect() // 获取小球的相当于适口的位置
          let x = rect.left - 32
          let y = -(window.innerHeight - rect.top - 22)
          el.style.display = ''
          el.style.webkitTransform = `translate3d(0, ${y}px, 0)`
          el.style.transform = `translate3d(0, ${y}px, 0)`
          let inner = el.getElementsByClassName('inner-hook')[0]
          inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`
          inner.style.transform = `translate3d(${x}px, 0, 0)`
        }
      }
    },
    enter (el, done) {
      /* eslint-disable no-unused-vars */
      let rf = el.offsetHeight
      this.$nextTick(() => {
        el.style.webkitTransform = 'translate3d(0, 0, 0)'
        el.style.transform = 'translate3d(0, 0, 0)'
        let inner = el.getElementsByClassName('inner-hook')[0]
        inner.style.webkitTransform = 'translate3d(0, 0, 0)'
        inner.style.transform = 'translate3d(0, 0, 0)'
        el.addEventListener('transitionend', done)
      })
    },
    afterEnter (el) {
      let ball = this.dropBalls.shift()
      if (ball) {
        ball.show = false
        el.style.display = 'none'
      }
    },
    toggleList () {
      if (!this.totalCount) {
        return
      }
      this.fold = !this.fold
    }
  },
  components: {
    cartcontrol
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
    .ball-container
      .ball
        position fixed
        left: 32px
        bottom: 22px
        z-index:200
        transition: all .6s cubic-bezier(0.49, -0.29, 0.75, 0.41)
        .inner
          width 16px
          height 16px
          border-radius 50%
          background rgb(0,160,220)
          transition: all .6s linear
    .shopcart-list
      position: absolute
      top: 0
      left: 0
      z-index: -1
      width: 100%
      
</style>
