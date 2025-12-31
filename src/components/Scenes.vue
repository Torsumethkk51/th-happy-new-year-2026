<script setup lang="ts">
import { Fireworks } from 'fireworks-js'
import { onMounted, onUnmounted, ref } from 'vue'

const container = ref<HTMLDivElement | null>(null)
type FireworksInstance = InstanceType<typeof Fireworks> 
let fireworks = defineModel<FireworksInstance | null>("fireworks")

onMounted(() => {
  if (!container.value) return

  fireworks.value = new Fireworks(container.value, {
    sound: {
      enabled: true,
      files: [
        'https://fireworks.js.org/sounds/explosion0.mp3',
        'https://fireworks.js.org/sounds/explosion1.mp3',
        'https://fireworks.js.org/sounds/explosion2.mp3'
      ],
      volume: {
        min: 10, // Increased from 1
        max: 30  // Increased from 1
      },
    },
    autoresize: true,
    opacity: 0.5,
    acceleration: 1.05,
    friction: 0.97,
    gravity: 1.5,
    particles: 50,
    traceLength: 3,
    traceSpeed: 10,
    explosion: 5,
    intensity: 30,
    flickering: 50,
    lineStyle: 'round',
    hue: { min: 0, max: 360 },
    delay: { min: 15, max: 30 },
    rocketsPoint: { min: 50, max: 50 }
  })
})

onUnmounted(() => {
  if (fireworks.value) {
    fireworks.value.stop()
  }
})
</script>

<template>
  <div class="scene-container">
    <div ref="container" class="fireworks-container" ></div>
    <slot></slot>
  </div>
</template> 

<style scoped>
  .scene-container {
    width: 100%;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    background-color: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(5px);
    padding: 2em 0;
    overflow: hidden;
    position: relative;
  }

  .fireworks-container {
    position: absolute;
    inset: 0;
  }
</style>