<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{ food.count }}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script>
import Vue from 'vue'
import { bus } from '../bus.js'

export default {
  name: 'cartcontrol',
  props: {
    food: {
      type: Object
    }
  },
  created () {
    this.$nextTick(() => {})
  },
  methods: {
    addCart (event1) {
      if (!event1._constructed) {
        return false
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }

      bus.$emit('cart-add', event1.target)
    },
    decreaseCart (event) {
      if (!event._constructed) {
        return false
      }
      if (this.food.count > 0) {
        this.food.count--
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" ref="stylesheet/stylus" scoped>
  .cartcontrol
    font-size: 0
    .cart-decrease
      display: inline-block
      padding: 6px
      transition: all 0.4s linear
      &.move-enter-to
        opacity: 1
        transform: translate3D(0, 0, 0)
      .inner
        display: inline-block
        line-height: 24px
        color: rgb(0, 160, 220)
        font-size: 24px
        transition: all 0.4s linear
        transform: rotate(0)
      &.move-enter, &.move-leave-to
        opacity: 0
        transform: translate3D(24px, 0, 0)
        .inner
          transform: rotate(180deg)
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      line-height: 24px
      text-align: center
      padding-top: 6px
      color: rgb(147, 153, 159)
      font-size: 10px
    .cart-add
      display: inline-block
      line-height: 24px
      color: rgb(0, 160, 220)
      font-size: 24px
      padding: 6px
</style>
