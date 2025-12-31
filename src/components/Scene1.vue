<script setup lang="ts">
import { ref, Transition } from 'vue';

let config = defineModel<{ 
  step: number, 
  isEnd: boolean, 
  isDisappear: boolean 
}>("config")
let currentScene = defineModel<number>("currentScene")
let username = defineModel<string>("username")
let headingMoved = ref(false)

function nextStep(delayInSec: number = 0) {
  setTimeout(() => {
    if (config.value) {
      config.value.step++
    }
  }, delayInSec * 1000)
}

function onStep1End() {
  nextStep(0.5)
}

function onStep2End() {
  if (headingMoved.value) return
  headingMoved.value = true
  nextStep()
}

function setDisapear() {
  if (config.value) {
    config.value.isDisappear = true
  }
}

function endThisScene() {
  if (config.value) {
    config.value.isEnd = true
  }
}

</script>

<template>
  <Transition name="dissolve" @after-leave="endThisScene">
    <div v-if="currentScene === 1 && (config && !config.isDisappear)">
      <div class="content">
        <Transition name="fade" @after-enter="onStep1End">
          <h1 
            v-if="config && config.step >= 1"
            :class="{ 'move-up': config && config.step >= 2 }"
            @transitionend="onStep2End"
          >
            สวัสดีปีใหม่ <span>2026</span>
          </h1>
        </Transition>
        <div class="name-input">
          <Transition name="fade">
            <input 
              v-if="config && config.step >= 3"
              type="text"
              placeholder="ชื่ออะไรอ่ะ บอกหน่อยสิ (ชื่อเล่นนะ)"
              v-model="username"
            >
          </Transition>
          <Transition name="fade">
            <button v-if="username && username.length > 0" @click="setDisapear">
              ไปต่อ
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
    font-size: 2.5em;
    transition: transform .25s;
  }

  span {
    color: rgb(255, 113, 52);
  }

  .move-up {
    transform: translateY(-50px);
  }

  .name-input {
    width: 100%;
    display: flex;
    gap: .5em;
    position: absolute;
    top: 2em;
  }

  input {
    flex: 1;
    padding: .5em 1em;
    font-size: 1em;
    border-radius: 10px;
    border: 0;
    box-shadow: 0 0 10px 0px rgba(0, 0, 0, .15);
    transition: outline .15s ease;
  }

  input:focus, input:not(:placeholder-shown) {
    outline: 3px solid rgb(255, 166, 0);
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