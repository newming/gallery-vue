<template>
  <div id="app">
    <image-card v-for='(item,i) in cardList' :key='i' :data='item' :info='cardInfo[i]' :index='i' @cardClick="cardClick"></image-card>
    <control :info='cardInfo' @controlClick='cardClick'></control>
  </div>
</template>

<script>
import ImageCard from './components/ImageCard'
import Control from './components/Control'
import cardList from './config'

export default {
  name: 'app',
  data () {
    return {
      cardList: cardList,
      cardInfo: [
        // {
        //   pos: {
        //     left: 0,
        //     top: 0
        //   },
        //   rotate: 0,
        //   active: false,
        //   back: false
        // }
      ], // 每个 card 的位置信息
      validPos: {
        centerPos: {
          left: 0,
          top: 0
        },
        hPosRange: {
          leftPos: [0, 0],
          rightPos: [0, 0],
          y: [0, 0]
        },
        topPos: {
          x: [0, 0],
          y: [0, 0]
        }
      } // 可选的摆放位置对象
    }
  },
  components: {
    ImageCard, Control
  },
  mounted () {
    // 计算可以排列区域
    this.computedPos()
  },
  methods: {
    computedPos () {
      let winW = document.documentElement.clientWidth
      let winH = document.documentElement.clientHeight
      let halfWinW = Math.floor(winW / 2)
      let halfWinH = Math.floor(winH / 2)
      let imgCardW = 260 // 一个卡片的宽高，这里直接用 css 设置的值了
      let imgCardH = 300
      let halfImgCardW = Math.floor(imgCardW / 2)
      let halfImgCardH = Math.floor(imgCardH / 2)
      // 中间点
      this.validPos.centerPos = {
        left: halfWinW - halfImgCardW,
        top: halfWinH - halfImgCardH
      }
      // 左右可选位置
      this.validPos.hPosRange = {
        leftPos: [-halfImgCardW, halfWinW - halfImgCardW * 3],
        rightPos: [halfWinW + halfImgCardW, winW - halfImgCardW],
        y: [-halfImgCardH, winH - halfImgCardH]
      }
      // 上方可选位置
      this.validPos.topPos = {
        x: [halfWinW - imgCardW, halfWinW + halfImgCardW],
        y: [-halfImgCardH, halfWinH - halfImgCardH * 3]
      }
      // console.log(this.validPos)
      // 将所有的可选点计算完毕后计算各个卡片位置
      this.reRender(0)
    },
    reRender (activeIndex) {
      let cardInfo = new Array(this.cardList.length)
      let {centerPos, hPosRange, topPos} = this.validPos
      // 生成中间图片位置信息
      let imageCenter = {
        pos: centerPos,
        rotate: 0,
        active: true,
        back: false
      }
      cardInfo.splice(activeIndex, 1) // 数组删除一位
      // 生成上边图片信息
      let imageTop = null
      let imgTopNum = Math.floor(Math.random() * 2)  // 放在上边的图片 0 或 1 张
      let imgTopIndex = 0
      if (imgTopNum) {
        imgTopIndex = Math.floor(Math.random() * cardInfo.length)
        imageTop = {
          pos: {
            left: this.getRandom(topPos.x[0], topPos.x[1]),
            top: this.getRandom(topPos.y[0], topPos.y[1])
          },
          rotate: this.getRotateRandom(),
          active: false,
          back: false
        }
        cardInfo.splice(imgTopIndex, imgTopNum)
      }
      // 生成左右两边
      for (let i = 0, j = cardInfo.length, k = j / 2; i < j; i++) {
        let vilidPos = null
        if (i < k) {
          vilidPos = hPosRange.leftPos
        } else {
          vilidPos = hPosRange.rightPos
        }
        cardInfo[i] = {
          pos: {
            left: this.getRandom(vilidPos[0], vilidPos[1]),
            top: this.getRandom(hPosRange.y[0], hPosRange.y[1])
          },
          rotate: this.getRotateRandom(),
          active: false,
          back: false
        }
      }
      // 将所有的重新放到 cardInfo
      if (imageTop) {
        cardInfo.splice(imgTopIndex, 0, imageTop)
      }
      cardInfo.splice(activeIndex, 0, imageCenter)
      // console.log(cardInfo)
      this.cardInfo = cardInfo
    },
    cardClick (i) {
      let imgInfo = this.cardInfo[i]
      if (imgInfo.active) {
        // 如果已经居中了，翻转
        this.cardInfo[i].back = !this.cardInfo[i].back
      } else {
        this.reRender(i)
      }
    },
    getRandom (min, max) {
      return Math.floor(Math.random() * (max - min) + min)
    },
    getRotateRandom () {
      // 返回正负 30 之间的数
      return Number((Math.random() > 0.5 ? '' : '-') + Math.floor(Math.random() * 30))
    }
  }
}
</script>

<style lang="stylus" rel='stylesheet/stylus'>
*
  margin 0
  padding 0
  box-sizing border-box

html, body
  width 100%
  height 100%
#app
  background-color #ddd
  height 100%
  position relative
  overflow hidden
</style>
