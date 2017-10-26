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
    <div class="foods-wrapper"></div>
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
</style>
