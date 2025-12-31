<script setup lang="ts">
import { computed, ref, Transition, watch } from 'vue';
import emailjs from "@emailjs/browser"

let config = defineModel<{ 
  step: number, 
  isEnd: boolean, 
  isDisappear: boolean 
}>("config")
let currentScene = defineModel<number>("currentScene")
let username = defineModel<string>("username")
let picPreview = ref<string>()
let headingMoved = ref(false)
let oldHeadingHide = ref(false)
let isStep3Disappear = ref(false)
let isMailSend = ref(false)
let message = ref("")

function sendEmail() {
  emailjs.init("kawdZ1izyPitFn--n")
  emailjs.send("service_txnjcye","template_co4i2hg",{
    name: username.value,
    message: message.value,
  })
  .then( () => {
    message.value = ""
    isMailSend.value = true
    alert("ส่งแล้ว! ขอบคุณสำหรับคำอวยพรน้า")
  })
  .catch(() => alert("ส่งล้มเหลว โปรดลองภายหลังน้า"));
}

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

function onStep3End(e: Event) {
  let file = (e.target as HTMLInputElement).files?.[0] ?? null
  if (!file) return
  picPreview.value = URL.createObjectURL(file)
  nextStep()
}

function onStep4End() {
  nextStep()
}

function onStep5End() {
  oldHeadingHide.value = true
  nextStep(1)
  endThisScene()
}

function endThisScene() {
  if (config.value) {
    config.value.isEnd = true
  }
}

watch(config, () => {
  if (config.value) {
    console.log(config.value.step)
  }
})

let buttonMsg = computed(() => {
  return isMailSend.value ? "ขอบคุณสำหรับคำอวยพรน้า" : "ส่งเมล"
})

</script>

<template>
  <Transition name="dissolve" @after-leave="endThisScene">
    <div v-if="currentScene === 2 && (config && !config.isDisappear)">
      <div class="content">
        <Transition name="fade" @after-enter="onStep1End" @after-leave="onStep5End">
          <h1 
            v-if="config && config.step >= 1 && config.step <= 4"
            :class="{ 'move-up': config && config.step >= 2 }"
            @transitionend="onStep2End"
          >
            สวัสดีปีใหม่นะคุณ <span>{{ username }}</span>
          </h1>
        </Transition>
        <Transition name="fade" @after-enter="">
          <h1 
            v-if="config && config.step >= 5 && oldHeadingHide" 
            :class="{ 'move-up': config && config.step >= 6 }"
          >
            <span>คำอวยพรปีใหม่</span>
          </h1>
        </Transition>
        <Transition name="fade" @after-leave="isStep3Disappear = true">
          <div v-if="config && config.step === 3" class="image-input">
            <h2>เลือกรูปที่อธิบายปีนี้มาหน่อยสิ</h2>
            <input 
              type="file" 
              id="imagePicker"
              @change="onStep3End"
              accept="image/*, image/heic, image/heif"
            >
            <label for="imagePicker">เลือกมาเลย</label>
          </div>
        </Transition>
        <Transition name="fade" @after-enter="onStep4End">
          <div v-if="config && config.step >= 4 && isStep3Disappear" class="image-box">
            <img :src="picPreview" alt="this year image">
          </div>
        </Transition>
        <Transition name="fade">
          <div v-if="config && config.step >= 5" class="wish-box">
            <h3>ขอให้ปีนี้เป็นปีที่ดีสำหรับคุณ {{ username }} นะ สิ่งไหนดีก็เก็บไว้ ไม่ดีก็ทิ้งไปในปีก่อนนะ ขอให้สุขภาพแข็งแรง คิดอะไรก็ประสบผลสำเร็จนะ <span>จาก ต่อ ถึง {{ username }}</span> ถ้าอยากอวยพรเจ้าของเว็บไซต์ก็พิมพ์มาได้นะ</h3>
            <textarea type="text" v-model="message" placeholder="อยากบอกอะไรก็พิมพ์มาเลย" :disabled="isMailSend"></textarea>
            <button @click="sendEmail" :disabled="isMailSend">{{ buttonMsg }}</button>
          </div>
        </Transition>
      </div>
    </div>
  </Transition>
</template> 

<style scoped>
  .content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: relative;
  }

  h1 {
    font-size: 2em;
    transition: transform .25s;
    text-align: center;
    text-shadow: 0 0 10px black;
  }

  span {
    color: rgb(255, 113, 52);
  }

  h2 {
    font-size: 1.25em;
    text-align: center;
    text-shadow: 0 0 10px black;
  }

  h3 {
    width: 300px;
    margin-top: 1.5em;
    text-align: center;
    text-shadow: 0 0 10px black;
  }

  .image-box {
    width: 300px;
    height: 400px;
    overflow: hidden;
    border-radius: 10px;
    box-shadow: 0 0 10px 10px rgba(255, 166, 0, 0.25);
  }

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }

  .move-up {
    transform: translateY(-20px);
  }

  .image-input {
    width: 100%;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 1em;
    padding: 1em;
  }

  textarea {
    max-width: 100%;
    min-width: 100%;
    max-height: 100px;
    min-height: 100px;
    resize: none;
    padding: .5em;
    font-size: 1em;
    font-family: "Kanit", system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    border-radius: 10px;
    border: 0;
    box-shadow: 0 0 10px 0px rgba(0, 0, 0, .15);
    transition: outline .15s ease;
  }

  textarea:focus, textarea:not(:placeholder-shown) {
    outline: 3px solid rgb(255, 166, 0);
  }

  input[type=file] {
    display: none;
  }

  label {
    width: 100%;
    padding: .5em;
    font-size: 1em;
    border-radius: 10px;
    border: 0;
    background-color: rgb(255, 113, 52);
    color: white;
    transition: background-color .15s ease;
    font-weight: 600;
    text-align: center;
  }

  label:hover {
    background-color: rgb(189, 79, 32);
  }
  label:active {
    background-color: rgb(255, 113, 52);
  }

  button {
    padding: .6em;
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

  .wish-box {
    display: flex;
    flex-direction: column;
    gap: 1em;
  }
</style>