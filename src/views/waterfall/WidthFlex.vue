<template>
  <div class="waterfall-width-flex">
    <div class="image-col" v-for="(col, index) in imgList" :key="index">
      <div class="image-box" v-for="img in col" :key="img">
        <img :src="img" alt="" />
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
import { set } from 'lodash-es'
const imgList = ref<any[]>([])
function loadImage() {
  for (let i = 0; i < 17; i++) {
    let colIndex = i % 4 //假设排4列，也可动态计算
    const imgSrc = `./../../assets/images/${i}.jpg`
    let url = new URL(imgSrc, import.meta.url).href
    // imgList.value.push(url)
    if (imgList.value[colIndex]) {
      imgList.value[colIndex].push(url)
    } else {
      set(imgList.value, colIndex, [url])
    }
  }
}
loadImage()
</script>
<style lang="scss" scoped>
.waterfall-width-flex {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  .image-col {
    width: 20%;

    .image-box {
      padding: 10px;
    }
  }
  img {
    display: block;
    width: 100%;
  }
}
</style>
