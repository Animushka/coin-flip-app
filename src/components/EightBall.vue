<template>
  <div class="eight-ball" @click="shakeBall" :class="{ shaking: isShaking }">
    <div class="eight-ball__circle">
      <span class="eight-ball__number">8</span>

      <div class="eight-ball__answer">
        <div class="eight-ball__triangle">
          <div class="eight-ball__answer-text" :class="{ visible: showAnswer }">
            {{ answer }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useI18n } from 'vue-i18n'

const { locale, messages } = useI18n();

const answer = ref<string>('')
const isShaking = ref(false)
const showAnswer = ref(false)

function shakeBall() {
  if (isShaking.value) return

  showAnswer.value = false
  isShaking.value = true

  setTimeout(() => {
    const arr = messages.value[locale.value].answers as string[];
    answer.value = arr[Math.floor(Math.random() * arr.length)]
    isShaking.value = false
    showAnswer.value = true
  }, 1000)
}
</script>

<style scoped>
.eight-ball {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
  user-select: none;
  cursor: pointer;
}

.eight-ball__circle {
  width: clamp(200px, 50vw, 300px);
  height: clamp(200px, 50vw, 300px);
  background: radial-gradient(circle at center, #111 60%, #222);
  border-radius: 50%;
  box-shadow: 0 6px 24px rgba(0, 0, 0, 0.6);
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.eight-ball__number {
  color: #fff;
  font-size: clamp(2rem, 6vw, 3rem);
  font-weight: 900;
  position: absolute;
  top: 1.2rem;
}

.eight-ball__answer {
  position: absolute;
  bottom: 25%;
  left: 50%;
  transform: translateX(-50%);
  width: 120px;
  height: 100px;
}

.eight-ball__triangle {
  width: 100%;
  height: 100%;
  background-color: #1976d2;
  clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.eight-ball__answer-text {
  color: #fff;
  font-size: 0.75rem;
  font-weight: bold;
  text-align: center;
  line-height: 1.1;
  opacity: 0;
  margin-top: 2rem;
  transition: opacity 0.4s ease;
  max-width: 40%;
  word-break: break-word;
  z-index: 1;
}

.eight-ball__answer-text.visible {
  opacity: 1;
}

.eight-ball.shaking .eight-ball__circle {
  animation: shake 1s;
}

@keyframes shake {
  0% { transform: translateX(0); }
  15% { transform: translateX(-10px); }
  30% { transform: translateX(10px); }
  45% { transform: translateX(-8px); }
  60% { transform: translateX(8px); }
  75% { transform: translateX(-5px); }
  90% { transform: translateX(5px); }
  100% { transform: translateX(0); }
}
</style>
