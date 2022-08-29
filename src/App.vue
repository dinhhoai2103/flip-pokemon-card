<template>
  <div>
    <main-screen v-if="statusScreen === 'default'" :modes="modes" @onStart="onStart" />
    <play-screen v-if="statusScreen === 'play'" :cardContext="setting.cardContext" @onFinish="onResult" />
    <result-screen v-if="statusScreen === 'result'" :timer="timer" @onStartAgain="statusScreen = 'default'" />
    <footer-screen v-if="statusScreen === 'play'" @onStartAgain="statusScreen = 'default'" />
  </div>
</template >

<script setup>
import { ref, reactive } from 'vue'
import MainScreen from './components/MainScreen.vue';
import PlayScreen from './components/PlayScreen.vue';
import ResultScreen from './components/ResultScreen.vue';
import FooterScreen from './components/FooterScreen.vue';
import { shuffleArray } from './utils/shuffleArray'
const statusScreen = ref('default')
const setting = reactive({
  totalOfBlocks: 0,
  cardContext: [],
  startedAt: null,
})
const timer = ref(0)
const modes = [
  {
    type: '4x4',
    name: 'Super Easy',
    total: 16,
  },
  {
    type: '6x6',
    name: 'Easy',
    total: 36,
  },
  {
    type: '8x8',
    name: 'Normal',
    total: 64,
  },
  {
    type: '10x10',
    name: 'Hard',
    total: 100,
  },
]
const onStart = (config) => {
  setting.totalOfBlocks = config.totalBlocks
  const firstCard = Array.from({ length: setting.totalOfBlocks / 2 }, (_, i) => i + 1)
  const secondCard = [...firstCard]
  const cards = [...firstCard, ...secondCard]
  setting.cardContext = shuffleArray(shuffleArray(shuffleArray(shuffleArray(cards))))
  setting.startedAt = new Date().getTime()
  statusScreen.value = 'play'
}
const onResult = () => {
  timer.value = new Date().getTime() - setting.startedAt
  statusScreen.value = 'result'
}

</script>
  