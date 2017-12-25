<template>
  <div class='card-wrap' :style='styleObj' :class='className' @click='clickHandler'>
    <img :src="data.img" :alt="data.title">
    <h2>{{ data.title }}</h2>
    <div class="back">
      <p>{{ data.desc }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'img-card',
  props: {
    data: {
      type: Object,
      default: () => {}
    },
    info: {
      type: Object,
      default: () => {}
    },
    index: {
      type: Number,
      default: 0
    }
  },
  computed: {
    styleObj () {
      let style = {}
      if (this.info && this.info.pos) {
        style = {
          top: this.info.pos.top + 'px',
          left: this.info.pos.left + 'px'
        }
      }
      if (this.info && !this.info.active) {
        style.transform = `rotate(${this.info.rotate}deg)`
      }
      return style
    },
    className () {
      let cls = ''
      if (this.info && this.info.active) {
        cls += 'center'
      }
      if (this.info && this.info.back) {
        cls += ' active'
      }
      return cls
    }
  },
  methods: {
    clickHandler () {
      this.$emit('cardClick', this.index)
    }
  }
}
</script>

<style lang="stylus" rel='stylesheet/stylus'>
.card-wrap
  position absolute
  width 260px
  height 300px
  background-color #fff
  padding 20px
  transform-origin: 0 50% 0
  overflow hidden
  transition all 0.6s ease-in-out
  box-shadow 0px 0px 17px 0px rgba(0,0,0,0.5)
  cursor pointer
  // 添加 active class 进行背面查看
  &.center
    z-index 1
  &.active
    transform translate(260px) rotateY(180deg)
    .back
      z-index 1
  img
    width 100%
  h2
    text-align center
    color #353535
    font-weight 300
    font-size 22px
    margin-top 8px
  .back
    position absolute
    top 0
    left 0
    width 100%
    height 100%
    background-color #fff
    color #888
    transform: rotateY(180deg) translateZ(1px);
    padding: 20px
    overflow hidden
    z-index -1
    line-height 1.5
    display flex
    align-items center
</style>
