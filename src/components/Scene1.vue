<script setup lang="ts">
import { onMounted, ref, Transition, useTemplateRef, watch, type Ref } from 'vue';

let currentScene = defineModel<number>("currentScene")
let username = defineModel<string>("username")
let headingMoved = ref(false)
let scene1Step = ref(0) // step is 0 = not start
let isEnd = ref(false)

function nextStep() {
  setTimeout(() => {
    scene1Step.value++
  }, 500)
}

function onHeadingMoveEnd() {
  if (headingMoved.value) return
  headingMoved.value = true
  nextStep()
}

function endThisScene() {
  if (!username.value || username.value.trim().length === 0) return
  isEnd.value = !isEnd.value
}

// step to 1 = start animation
onMounted(() => {
  scene1Step.value++
})

</script>

<template>
  <Transition name="dissolve" @after-leave="currentScene && currentScene++">
    <div v-if="currentScene === 1 && !isEnd">
      <div class="content">
        <Transition name="fade" @after-enter="nextStep">
          <h1 
            v-if="scene1Step >= 1"
            :class="{ 'move-up': scene1Step >= 2 }"
            @transitionend="onHeadingMoveEnd"
          >
            สวัสดีปีใหม่ <span>2026</span>
          </h1>
        </Transition>
        <div class="name-input">
          <Transition name="fade">
            <input 
              v-if="scene1Step >= 3"
              type="text" 
              placeholder="ชื่ออะไรอะบอกหน่อยสิ"
              v-model="username"
            >
          </Transition>
          <Transition name="fade">
            <button v-if="username && username.length > 0" @click="endThisScene">
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

  span {
    color: rgb(255, 113, 52);
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