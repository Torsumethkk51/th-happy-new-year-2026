<script setup lang="ts">

</script>

<template>
  <div class="scene-container">
    <slot></slot>
  </div>
</template> 

<style scoped>
  .scene-container {
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    background-color: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(5px);
  }
</style>