<template>
  <textarea rows="" cols="" class="textarea" v-model="content"></textarea>
  <van-button @click="clickBtn">复制</van-button>
</template>

<script setup>
import { ref } from 'vue'
import useClipboard from 'vue-clipboard3'

let content = ref('')

const { toClipboard } = useClipboard()
const copy = async (Msg) => {
  try {
    //复制
    await toClipboard(Msg)
    console.log(Msg)
    //下面可以设置复制成功的提示框等操作
    //...
  } catch (e) {
    //复制失败
    console.error(e)
  }
}

function clickBtn() {
  let contentArr = content.value.split('')
  let str = ''
  let colorArr = gradient('#0082FF', '#FF4A66', contentArr.length)
  console.log(colorArr)
  for (let i = 0; i < contentArr.length; i++) {
    const element = contentArr[i]
    str += `<color=${colorArr[i]}>${element}</color>`
  }
  copy(str)
}

//hex to rgb
function hexToRgb(hex) {
  var rgb = []
  for (var i = 1; i < 7; i += 2) {
    rgb.push(parseInt('0x' + hex.slice(i, i + 2)))
  }
  return rgb
}

function rgbToHex(r, g, b) {
  var hex = ((r << b) | (g << 8) | b).toString(16)
  return '#' + new Array(Math.abs(hex.length - 7)).join('0') + hex
}

//计算渐变过渡色
function gradient(startColor, endColor, step) {
  //将hex转换为rgb
  var sColor = hexToRgb(startColor),
    eColor = hexToRgb(endColor)
  //计算R/G/B每一步差值
  var rStep = (eColor[0] - sColor[0]) / step,
    gStep = (eColor[1] - sColor[1]) / step,
    bStep = (eColor[2] - sColor[2]) / step
  var gradientColor = []
  for (var i = 0; i < step; i++) {
    gradientColor.push(
      rgbToHex(
        parseInt(
          rStep * i + sColor[0],
          parseInt(gStep * i + sColor[1], parseInt(bStep * i + sColor[2]))
        )
      )
    )
  }
  return gradientColor
}
</script>
<style lang="scss" scoped>
.textarea {
  border: 0;
  border: 1px solid #000;
  border-radius: 5px;
  background-color: rgba(241, 241, 241, 0.98);
  resize: none;
  margin-bottom: 20px;
  height: 20vh;
  width: 100%;
  padding: 20px;
  -webkit-user-select: auto;
}
</style>
