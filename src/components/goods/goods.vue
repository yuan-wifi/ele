<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item in goods" :key="item.id" class="menu-item">
          <span class="name"><span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>{{ item.name }}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script>
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
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
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
    bottom: 58px
    width: 100%
    .menu-wrapper
      flex: 0 0 80px
      background: #f3f5f7
      width: 80px
      .menu-item
        .icon
          vertical-align: top
          display: inline-block
          width: 12px
          height: 12px
          margin-right: 4px
          background-size: 12px 12px
          background-repeat: no-repeat
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
    .foods-wrapper
      flex: 1
</style>
