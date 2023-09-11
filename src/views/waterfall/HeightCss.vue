<template>
  <div class="waterfall-height-css">
    <div class="image-box" v-for="img of imgList" :key="img.url">
      <img :src="img.url" />
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'

const imgList = ref<any[]>([])

function loadImage() {
  for (let i = 0; i < 17; i++) {
    let image = new Image()
    // let url = require(`@/assets/images/${i}.jpg`)
    const imgSrc = `./../../assets/images/${i}.jpg`
    let url = new URL(imgSrc, import.meta.url).href
    image.src = url
    image.onload = () => {
      imgList.value.push({
        url: url,
        width: image.width,
        height: image.height
      })
    }
  }
}
loadImage()
</script>
<style lang="scss" scoped>
.waterfall-height-css {
  display: flex;
  flex-wrap: wrap;
  .image-box {
    margin: 5px;
    flex-grow: 1;
  }
  img {
    display: block;
    min-width: 100%;
    height: 200px;
    object-fit: cover;
  }
  &:after {
    content: '';
    display: block;
    flex-grow: 99999;
  }
}
</style>
