<template>
  <div ref="waterfall" class="waterfall-height-js">
    <div class="row" v-for="(list, index) in imgList" :key="index">
      <div class="image-box" v-for="img in list" :key="img.url">
        <img :src="img.url" :height="img.height" />
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, reactive, onMounted, onBeforeUnmount } from 'vue'

import { throttle, set } from 'lodash-es'

const state = reactive({
  resizeRender: null,
  baseHeight: 200, //图片的基础计算高度
  // imgList: [[]], //用二维数据保存每一行数据
  rowWidth: 0, //每行的图片宽度
  rowCount: 0 //每行的索引
})
const imgList = ref<any[]>([[]])
const waterfall = ref<HTMLElement | null>(null)

//缩放后的总图片宽度与屏幕宽度比较
function compare(image: any) {
  //容器宽度
  // let clientWidth = this.$refs.waterfall.clientWidth
  let clientWidth = waterfall.value?.clientWidth!
  //计算每行宽度
  state.rowWidth += image.width
  console.log(state.rowWidth, 'aaaa')
  //如果宽度大于容器宽度，去掉多余的宽度，整体进行缩放适应容器让右边对齐
  if (state.rowWidth > clientWidth) {
    //减去每个css padding边距
    clientWidth = clientWidth - imgList.value[state.rowCount].length * 10
    state.rowWidth = state.rowWidth - image.width
    //把高度调整为放大后的
    let growAfterHeight = (clientWidth * state.baseHeight) / state.rowWidth
    imgList.value[state.rowCount].forEach((item: any) => {
      item.height = growAfterHeight
    })
    //把多余图片放入到下一行
    state.rowWidth = image.width
    state.rowCount++
    // this.$set(this.imgList, this.rowCount, [image])
    set(imgList.value, state.rowCount, [image])
  } else {
    imgList.value[state.rowCount].push(image)
  }
}

function loadImage() {
  for (let i = 0; i < 17; i++) {
    let image = new Image()
    const imgSrc = `./../../assets/images/${i}.jpg`
    let url = new URL(imgSrc, import.meta.url).href
    image.src = url
    image.onload = () => {
      compare({
        url: url,
        width: state.baseHeight * (image.width / image.height),
        height: state.baseHeight
      })
    }
  }
}

function resize() {
  //将已存在的图片数据展开，重新计算
  let newList = imgList.value.reduce((list, item) => list.concat(item), [])
  //清空数据
  imgList.value = [[]]
  state.rowWidth = 0
  state.rowCount = 0
  newList.forEach((image: any) => compare(image))
}

loadImage()
onMounted(() => {
  state.resizeRender = throttle(resize, 200)
  window.addEventListener('resize', state.resizeRender!)
})
onBeforeUnmount(() => {
  window.removeEventListener('resize', state.resizeRender!)
})
</script>
<style lang="scss" scoped>
.image-box {
  float: left;
  padding: 5px;
  img {
    display: block;
  }
}
</style>
