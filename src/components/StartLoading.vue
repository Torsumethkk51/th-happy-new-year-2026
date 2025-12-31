<script setup lang="ts">

let isStart = defineModel("isStart")
let { startAnimation } = defineProps({
  "startAnimation": Function
})

function ready() {
  if (startAnimation) {
    isStart.value = true
    startAnimation()
  }
}
</script>

<template>
  <div v-if="!isStart" class="overlay">
    <h1>เอาล่ะปีใหม่แล้ว พร้อมจะเริ่มปีนี้หรือยัง</h1>
    <button @click="ready">พร้อมแล้ว !!</button>
  </div>
</template>

<style scoped>
  .overlay {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    gap: 1em;
    width: 100%;
    height: 100vh;
    position: absolute;
    inset: 0;
    background-color: rgba(0, 0, 0, 0.666);
    backdrop-filter: blur(10px);
    color: white;
  }

  h1 {
    font-size: 1.25em;
    transition: transform .25s;
  }

  button {
    padding: .5em;
    font-size: 1.25em;
    border-radius: 10px;
    border: 0;
    background-color: rgb(255, 113, 52);
    color: white;
    transition: all .15s ease;
    font-weight: 600;
  }

  button:hover {
    opacity: .8;
  }
  button:active {
    opacity: .5;
  }
</style>