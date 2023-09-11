<template>
  <div ref="waterfall" class="waterfall-width-position">
    <div
      class="image-item"
      v-for="(img, index) in imgList"
      :key="index"
      :style="{ top: img.top + 'px', left: img.left + 'px' }">
      <div class="image">
        <img :src="img.url" alt="" />
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, reactive, onMounted } from 'vue'

const state = reactive<{
  colWidth: number
  colNumbers: number
  colHeight: any[]
}>({
  colWidth: 200,
  colNumbers: 0,
  colHeight: []
})

const imgList = ref<any[]>([])
const waterfall = ref<HTMLElement | null>(null)

//计算图片列数
function getColNumbers() {
  // let clientWidth = this.$refs.waterfall.clientWidth
  let clientWidth = waterfall.value?.clientWidth!
  state.colNumbers = Math.floor(clientWidth / state.colWidth)
}

function render(imgInfo: any) {
  console.log(imgInfo, 'imgInfo')
  let colIndex = imgInfo.index % state.colNumbers
  imgInfo.left = colIndex * state.colWidth
  //首行 top为 0，记录每列的高度
  if (imgInfo.index < state.colNumbers) {
    imgInfo.top = 0
    state.colHeight[colIndex] = state.colWidth / imgInfo.ratio
  } else {
    //获取高度的最小值
    let minHeight = Math.min.apply(null, state.colHeight)
    let minIndex = state.colHeight.indexOf(minHeight)
    //此图片的 top 为上面图片的高度，left 相等
    imgInfo.top = minHeight
    imgInfo.left = minIndex * state.colWidth
    //把高度加上去
    state.colHeight[minIndex] += state.colWidth / imgInfo.ratio
  }
  imgList.value.push(imgInfo)
}

function loadImage() {
  getColNumbers()
  for (let i = 0; i < 17; i++) {
    let image = new Image()
    const imgSrc = `./../../assets/images/${i}.jpg`
    let url = new URL(imgSrc, import.meta.url).href
    image.src = url
    image.onload = () => {
      render({
        index: i,
        url: url,
        ratio: image.width / image.height
      })
    }
  }
}
onMounted(() => {
  loadImage()
})
</script>
<style lang="scss" scoped>
.waterfall-width-position {
  position: relative;
  .image-item {
    position: absolute;
    width: 200px;
  }
  .image {
    padding: 5px;
  }
  img {
    display: block;
    width: 100%;
  }
}
</style>
