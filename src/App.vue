<template>
  <div>
    <div class="lang-switcher">
      <button @click="$i18n.locale = 'en'">EN</button>
      <button @click="$i18n.locale = 'ru'">RU</button>
    </div>
    <div class="tabs">
      <button
        :class="{ active: selectedGame === 'coin' }"
        @click="selectedGame = 'coin'"
      >
        {{ $t('coinFlip') }}
      </button>
      <button
        :class="{ active: selectedGame === '8ball' }"
        @click="selectedGame = '8ball'"
      >
        {{ $t('eightBall') }}
      </button>
    </div>
    <CoinFlip v-if="selectedGame === 'coin'" />
    <EightBall v-else />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import CoinFlip from './components/CoinFlip.vue';
import EightBall from './components/EightBall.vue';

const selectedGame = ref('coin');
</script>

<style scoped>
.lang-switcher {
  position: fixed;
  top: 1em;
  left: 1em;
  z-index: 10;
}

.tabs {
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 50%;
  display: flex;
  justify-content: center;
  box-shadow: 0 -2px 8px rgba(0,0,0,0.04);
}

.tabs button {
  flex: 1 1 0;
  min-width: 100px;
  padding: 0.5em 1em;
  border: none;
  background: black;
  cursor: pointer;
  font-size: 1em;
  margin: 0 0.25em;
  border-radius: 0.5em 0.5em 0 0;
  transition: background 0.2s;
}

.tabs button.active {
  background: #42b883;
  color: white;
  font-weight: bold;
}

.tabs button:not(.active):hover {
  background: #d3f9e6;
}

@media (max-width: 600px) {
  .tabs {
    padding: 0.25em 0;
  }
  .tabs button {
    font-size: 0.95em;
    min-width: 80px;
    padding: 0.5em 0.5em;
    margin: 0 0.15em;
  }
  .lang-switcher {
    top: 0.5em;
    left: 0.5em;
  }
}
</style>
