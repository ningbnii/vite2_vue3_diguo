<template>
  <div class="color">
    <Sketch v-model="colors"></Sketch>
    <Sketch v-model="colors2"></Sketch>
  </div>
  <textarea rows="" cols="" class="textarea" v-model="content"></textarea>
  <van-button @click="clickBtnGradient">复制</van-button>
</template>

<script>
import { Sketch } from '@ckpack/vue-color'
export default {
  components: {
    Sketch,
  },
}
</script>

<script setup>
import { ref } from 'vue'
import useClipboard from 'vue-clipboard3'
import { Toast } from 'vant'
import colorGradient from 'javascript-color-gradient'

let colors = ref('#ff0000')
let colors2 = ref('#ff0000')
let content = ref('')

const { toClipboard } = useClipboard()
const copy = async (Msg) => {
  try {
    //复制
    await toClipboard(Msg)
    Toast.success('复制成功')
    //下面可以设置复制成功的提示框等操作
    //...
  } catch (e) {
    //复制失败
    console.error(e)
  }
}

function clickBtnGradient() {
  let contentArr = content.value.split('，')
  let str = ''
  let color1 = colors.value.hex
  if (color1 == undefined) {
    Toast.fail('选择颜色')
    return
  }
  let color2 = colors2.value.hex
  if (color2 == undefined) {
    Toast.fail('选择颜色')
    return
  }

  const colorArr = colorGradient
    .setGradient(color1, color2)
    .setMidpoint(contentArr.length)
    .getArray()

  for (let i = 0; i < contentArr.length; i++) {
    const element = contentArr[i]
    str += `<color=${colorArr[i]}>${element}</color>`
  }
  copy(str)
}

function clickBtn() {
  let str = ''
  let color = colors.value.hex
  if (color == undefined) {
    Toast.fail('选择颜色')
    return
  }
  str += `<color=${color}>${content.value}</color>`
  copy(str)
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
.color {
  display: flex;
}
</style>
