<template>
  <div class="container">
    <div class="result-wrapper">
      <p v-if="!flipping && result">Результат: {{ result }}</p>
    </div>
    <div class="coin-wrapper">
      <div class="coin" :style="{ transform: `rotateY(${rotation}deg)` }">
        <div class="side front">
          <img src="/heads.png" alt="Орел" />
        </div>
        <div class="side back">
          <img src="/tails.png" alt="Решка" />
        </div>
      </div>
    </div>

    <button :disabled="flipping" @click="flipCoin">
      {{ flipping ? 'Бросаем...' : 'Подбросить' }}
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const rotation = ref(0)
const flipping = ref(false)
const result = ref<'Орел' | 'Решка' | null>(null)

function flipCoin() {
  flipping.value = true
  result.value = null

  const baseAngle = Math.random() < 0.5 ? 0 : 180
  const fullRotations = 360 * 4

  rotation.value += fullRotations + baseAngle

  // показываем результат после окончания анимации
  setTimeout(() => {
    const finalAngle = rotation.value % 360
    result.value = finalAngle === 180 ? 'Решка' : 'Орел'
    flipping.value = false
  }, 1500)
}

</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 5vh auto 0;
  padding: 0 1rem;
  max-width: 600px;
  text-align: center;
}

.result-wrapper {
  min-height: 6em; /* или clamp(...) если хочешь адаптив */
}

.coin-wrapper {
  width: clamp(120px, 30vw, 200px);
  height: clamp(120px, 30vw, 200px);
  perspective: 1000px;
}

.coin {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 1.5s ease-in-out;
}

.side {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  overflow: hidden;
  backface-visibility: hidden;
}

.front {
  transform: rotateY(0deg);
}

.back {
  transform: rotateY(180deg);
}

.coin img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

button {
  padding: 0.75em 1.5em;
  margin-top: 1em;
  font-size: clamp(1rem, 2.5vw, 1.25rem);
  cursor: pointer;
  border: none;
  border-radius: 0.5em;
  background-color: #3498db;
  color: white;
  transition: background-color 0.3s ease;
}

button:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
}

p {
  font-size: clamp(1rem, 2.5vw, 1.5rem);
}
</style>