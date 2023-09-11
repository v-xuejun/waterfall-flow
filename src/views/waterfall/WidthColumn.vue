<template>
  <div class="waterfall-width-column">
    <div class="image-box" v-for="img in imgList" :key="img">
      <img :src="img" alt="" />
    </div>
  </div>
</template>
<script lang="ts" setup>
import { onMounted, ref } from 'vue'

const imgList = ref<any[]>([])
const cols = ref(3)
function loadImage() {
  for (let i = 0; i < 17; i++) {
    const imgSrc = `./../../assets/images/${i}.jpg`
    let url = new URL(imgSrc, import.meta.url).href
    imgList.value.push(url)
  }
}
loadImage()
onMounted(() => {
  window.addEventListener('resize', () => {
    cols.value++
  })
})
</script>
<style lang="scss" scoped>
.waterfall-width-column {
  column-count: 3;
  column-gap: 10px;
  .image-box {
    margin-bottom: 10px;
    img {
      display: block;
      width: 100%;
    }
  }
}
@media (min-width: 768px) {
  .waterfall-width-column {
    column-count: 3;
  }
}

@media (min-width: 992px) {
  .waterfall-width-column {
    column-count: 4;
  }
}

@media (min-width: 1200px) {
  .waterfall-width-column {
    column-count: 6;
  }
}
</style>
