<template>
  <div style="margin-left: 100px">
    <div class="box" v-for="value of values" :key="value">
      <img
        class="img"
        :style="{ 'object-fit': value }"
        src="https://picsum.photos/id/1027/200/300" />
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'

const values = ref<any[]>(['fill', 'contain', 'cover', 'none', 'scale-down'])
</script>

<style lang="scss" scoped>
.box {
  margin-left: 20px;
  float: left;
  width: 150px;
  height: 150px;
  border: 1px solid red;
}
img {
  width: 100%;
  height: 100%;
}
</style>
