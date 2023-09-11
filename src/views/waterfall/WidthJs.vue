<template>
  <div ref="waterfall" class="waterfall-width-js">
    <div ref="refContainer" class="container">
      <div class="image-col" v-for="(col, index) in imgList" :key="index">
        <div class="image-box" v-for="img in col" :key="img">
          <img :src="img" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, reactive, onMounted, onBeforeUnmount } from 'vue'

import { throttle, set } from 'lodash-es'

const state = reactive({
  resizeRender: null,
  colWidth: 200,
  colNumbers: 0
})

const imgList = ref<any[]>([])
const waterfall = ref<HTMLElement | null>(null)
const refContainer = ref<HTMLElement | null>(null)

//计算图片列数
function getColNumbers() {
  // let clientWidth = this.$refs.waterfall.clientWidth
  let clientWidth = waterfall.value?.clientWidth!
  state.colNumbers = Math.floor(clientWidth / state.colWidth)
  console.log(state.colNumbers, 'state.colNumbers', clientWidth)
  refContainer.value?.style.setProperty(
    'margin-left',
    (clientWidth - state.colWidth * state.colNumbers) / 2 + 'px'
  )
  // this.$refs.container.style.marginLeft =
  //   (clientWidth - this.colWidth * this.colNumbers) / 2 + 'px'
}

function loadImage() {
  getColNumbers()

  for (let i = 0; i < 17; i++) {
    let colIndex = i % state.colNumbers
    let imgSrc = `./../../assets/images/${i}.jpg`
    let url = new URL(imgSrc, import.meta.url).href
    if (imgList.value[colIndex]) {
      imgList.value[colIndex].push(url)
    } else {
      set(imgList.value, colIndex, [url])
    }
  }
}

function resize() {
  imgList.value = []
  loadImage()
}

onMounted(() => {
  loadImage()
  state.resizeRender = throttle(resize, 200)
  window.addEventListener('resize', state.resizeRender!)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', state.resizeRender!)
})
</script>
<style lang="scss" scoped>
.waterfall-width-js {
  width: 100%;
  margin: 0 auto;
  overflow: hidden;
}
.image-col {
  float: left;
  width: 200px;
  .image-box {
    margin: 10px 5px;
  }
  img {
    display: block;
    width: 100%;
  }
}
</style>
