<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
import Scenes from './components/Scenes.vue';
import Scene1 from './components/Scene1.vue';
import StartLoading from './components/StartLoading.vue';

let isStart = ref(false)
let username = ref("")
let currentScene = ref(parseInt(localStorage.getItem("currentScene")!) || 1)

// watch(currentScene, () => {
//   localStorage.setItem(
//     "currentScene",
//     currentScene.value.toString()
//   )
// })

</script>
  
<template>
  <Transition name="dissolve">
    <StartLoading v-model:is-start="isStart" />
  </Transition>
  <Transition name="fade">
    <template v-if="isStart">
      <Scenes>
        <Scene1 
          v-model:current-scene="currentScene" 
          v-model:username="username"
        />
      </Scenes>
    </template>
  </Transition>
</template>

<style scoped>
  .container {
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>