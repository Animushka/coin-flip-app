<template>
    <div class="container">
        <h1>Орел или Решка 🪙</h1>

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

        <p v-if="!flipping && result">Результат: {{ result }}</p>
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

    const isTails = Math.random() < 0.5
    const baseAngle = isTails ? 180 : 0
    const fullRotations = 360 * 4 // 4 оборота

    // Добавляем к предыдущему значению, чтобы сохранить направление
    rotation.value += fullRotations + baseAngle

    // Устанавливаем результат после завершения анимации
    setTimeout(() => {
        result.value = isTails ? 'Решка' : 'Орел'
        flipping.value = false
    }, 1500)
}
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 80px;
}

.coin-wrapper {
    width: 150px;
    height: 150px;
    perspective: 1000px;
    margin: 40px 0;
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
    padding: 12px 24px;
    font-size: 18px;
    cursor: pointer;
}
</style>