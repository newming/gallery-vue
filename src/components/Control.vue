<template>
  <div class='control-wrap'>
    <span v-for='(item, i) in info' :key='i' :class='{active: item.active}' @click="clickHandler(i)">{{ !item.active ? '' : item.back ? '反' : '正' }}</span>
  </div>
</template>

<script>
export default {
  name: 'control',
  props: {
    info: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    clickHandler (i) {
      this.$emit('controlClick', i)
    }
  }
}
</script>

<style lang="stylus" rel='stylesheet/stylus'>
.control-wrap
  position absolute
  bottom 30px
  width 100%
  text-align center
  display flex
  justify-content center
  align-items center
  span
    display block
    background-color rgba(0, 0, 0, 0.4)
    width 28px
    height 28px
    border-radius 50%
    margin 0 5px
    line-height 28px
    font-size 16px
    color #fff
    cursor pointer
    &.active
      transform scale(1.2)
</style>
