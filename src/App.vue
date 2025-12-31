<script setup lang="ts">
import { reactive, ref, watch } from 'vue';
import Scenes from './components/Scenes.vue';
import Scene1 from './components/Scene1.vue';
import StartLoading from './components/StartLoading.vue';
import Scene2 from './components/Scene2.vue';
import { Fireworks } from 'fireworks-js';

let isStart = ref(false)
let username = ref("")
let audio = new Audio("/bgm.mp3")
audio.loop = true
let currentScene = ref(1)
const startDelay = 750
let animationConfig = reactive({
  scene1: {
    step: 0,
    isEnd: false,
    isDisappear: false
  },
  scene2: {
    step: 0,
    isEnd: false,
    isDisappear: false
  }
})
type FireworksInstance = InstanceType<typeof Fireworks>
const fireworks = ref<FireworksInstance | null>()

function startAnimation() {
  audio.play()
  setTimeout(() => {
    animationConfig.scene1.step = 1
    fireworks.value?.start()
  }, startDelay)
}

function changeScene(scene: number) {
  currentScene.value = scene
}

watch(fireworks, () => fireworks.value?.start())

watch(animationConfig, () => {
  if (animationConfig.scene1.isEnd && animationConfig.scene2.step < 1) {
    changeScene(2)
    setTimeout(() => {
      animationConfig.scene2.step = 1
    }, 500)
  }
}, { deep: true })

</script>
  
<template>
  <Transition name="dissolve">
    <StartLoading 
      v-model:is-start="isStart"
      :start-animation="startAnimation"
    />
  </Transition>
  <Transition name="fade">
    <template v-if="isStart">
      <Scenes v-model:fireworks="fireworks">
        <Scene1 
          v-model:config="animationConfig.scene1"
          v-model:current-scene="currentScene" 
          v-model:username="username"
        />
        <Scene2
          v-model:config="animationConfig.scene2"
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