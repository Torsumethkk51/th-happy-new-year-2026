<script setup lang="ts">
import { onMounted, ref, Transition, useTemplateRef, watch, type Ref } from 'vue';

let currentScene = defineModel<number>("currentScene")
let username = defineModel<string>("username")
let headingMoved = ref(false)
let step = ref(parseInt(localStorage.getItem("scene1Step")!) || 0) // step is 0 = not start

function nextStep() {
  setTimeout(() => {
    step.value++
  }, 500)
}

function onHeadingMoveEnd() {
  if (headingMoved.value) return
  headingMoved.value = true
  nextStep()
}

// step to 1 = start animation
onMounted(() => {
  step.value++
})

// watch(step, () => {
//   localStorage.setItem(
//     "scene1Step",
//     step.value.toString()
//   )
// })

</script>

<template>
  <Transition name="dissolve">
    <div v-if="currentScene === 1" ref="parent">
      <div class="content">
        <Transition name="fade" @after-enter="nextStep">
          <h1 
            v-if="step >= 1"
            :class="{ 'move-up': step >= 2 }"
            @transitionend="onHeadingMoveEnd"
          >
            สวัสดีปีใหม่ 2026
          </h1>
        </Transition>
        <div class="name-input">
          <Transition name="fade">
            <input 
              v-if="step >= 3"
              type="text" 
              placeholder="ชื่ออะไรอะบอกหน่อยสิ"
              v-model="username"
            >
          </Transition>
          <Transition name="fade">
            <button v-if="username && username.length > 0" @click="currentScene++">
              ไปกันเลย
            </button>
          </Transition>
        </div>
      </div>
    </div>
  </Transition>
</template> 

<style scoped>
  .content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    position: relative;
  }

  h1 {
    font-size: 3em;
    transition: transform .25s;
  }

  .move-up {
    transform: translateY(-50px);
  }

  .name-input {
    width: 100%;
    display: flex;
    gap: .75em;
    position: absolute;
    top: 3em;
  }

  input {
    flex: 1;
    padding: .5em;
    font-size: 1em;
    border-radius: 10px;
    border: 0;
    box-shadow: 0 0 10px 0px rgba(0, 0, 0, .15);
    transition: .15s;
  }

  button {
    padding: .5em;
    font-size: 1em;
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