<template>
  <span v-if="type > -1" class="icon" :class="[classMap[type], sizeType , rightType, fontSize]"></span>
</template>

<script>

export default {
  name: 'iconType',
  props: {
    type: {
      type: Number
    },
    size: {
      type: Number
    },
    right: {
      type: Number
    },
    font: {
      type: Number
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  computed: {
    sizeType () {
      return 'icon-' + this.size
    },
    rightType () {
      return 'right-' + this.right
    },
    fontSize () {
      return 'font-' + this.font
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  
  .icon
    vertical-align: top
    display: inline-block
    background-repeat: no-repeat
    &.font-12
      width: 12px
      height: 12px
      font-size: 12px
      background-size: 12px 12px
    &.font-16
      width: 16px
      height: 16px
      font-size: 16px
      background-size: 16px 16px
    &.right-2
      margin-right: 2px
    &.right-4
      margin-right: 4px
    &.right-6
      margin-right: 6px
    &.icon-3
      &.decrease 
        bg-image('decrease_3')
      &.discount
        bg-image('discount_3')
      &.guarantee
        bg-image('guarantee_3')
      &.invoice
        bg-image('invoice_3')
      &.special
        bg-image('special_3')
    &.icon-1
      &.decrease 
        bg-image('decrease_1')
      &.discount
        bg-image('discount_1')
      &.guarantee
        bg-image('guarantee_1')
      &.invoice
        bg-image('invoice_1')
      &.special
        bg-image('special_1')

</style>