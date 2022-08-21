<template>
  <main-screen v-if="statusScreen === 'default'" :modes="modes" @onStart="onStart" />
  <play-screen v-if="statusScreen === 'play'" :cardContext="setting.cardContext" @onFinish="onResult" />
  <result-screen v-if="statusScreen === 'result'" :timer="timer" @onStartAgain="statusScreen = 'default'" />
  <footer-screen v-if="statusScreen === 'play'" @onStartAgain="statusScreen = 'default'" />
</template >

<script>
import MainScreen from './components/MainScreen.vue';
import PlayScreen from './components/PlayScreen.vue';
import ResultScreen from './components/ResultScreen.vue';
import FooterScreen from './components/FooterScreen.vue';
import { shuffleArray } from './utils/shuffleArray'
export default {
  name: 'App',
  data() {
    return {
      statusScreen: 'default',
      setting: {
        totalOfBlocks: 0,
        cardContext: [],
        startedAt: null,
      },
      timer: 0,
      modes: [
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
    }
  },
  methods: {
    onStart(config) {
      this.setting.totalOfBlocks = config.totalBlocks
      const firstCard = Array.from({ length: this.setting.totalOfBlocks / 2 }, (_, i) => i + 1)
      const secondCard = [...firstCard]
      const cards = [...firstCard, ...secondCard]
      this.setting.cardContext = shuffleArray(shuffleArray(shuffleArray(shuffleArray(cards))))
      this.setting.startedAt = new Date().getTime()
      this.statusScreen = 'play'
    },
    onResult() {
      this.timer = new Date().getTime() - this.setting.startedAt
      this.statusScreen = 'result'
    },
  },
  components: {
    MainScreen,
    PlayScreen,
    ResultScreen,
    FooterScreen
  }
}
</script>
  