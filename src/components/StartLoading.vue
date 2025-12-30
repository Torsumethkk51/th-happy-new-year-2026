<script setup lang="ts">
import { onMounted } from 'vue';

  let isStart = defineModel("isStart")

  onMounted(() => {
    setTimeout(() => {
      isStart.value = true
    }, 200)
  })
</script>

<template>
  <div v-if="!isStart" class="overlay"></div>
</template>

<style scoped>
  .overlay {
    width: 100%;
    height: 100vh;
    position: absolute;
    inset: 0;
    background-color: rgba(0, 0, 0, 0.666);
    backdrop-filter: blur(10px);
  }
</style>