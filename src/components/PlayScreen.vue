<template>
  <div class="screen">
    <div class="screen__inner" :style="{
      width: `${(((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3 / 4) + 16) * Math.sqrt(cardContext.length))}px`
      // ((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3 / 4 + margin right 1rem * item rows length
    
    }">
      <cart-item v-for="(card, index) in cardContext" :key="index" :ref="el => cards[index] = el"
        :cardUrl="`images/${card}.png`" :card="{ index, value: card }" @onFlip="checkRules" :rules="rules"
        :cardContext="cardContext" />
    </div>
  </div>
</template>

<script setup>
import CartItem from './CartItem.vue';
import { reactive, ref } from 'vue'
const cards = reactive([]);
const props = defineProps({
  cardContext: {
    type: Array,
    default: function () {
      return []
    },
  }
})
const emit = defineEmits(['onFinish'])
const rules = ref([])
const checkRules = (card) => {
  if (rules.value.length === 2) {
    return false;
  } else {
    rules.value.push(card)
  }
  if (rules.value.length === 2 && rules.value[0].value === rules.value[1].value) {
    cards[rules.value[0].index].onDisableCard()
    cards[rules.value[1].index].onDisableCard()
    rules.value = []
    const disabledCards = document.querySelectorAll('.screen .card.disabled')
    if (disabledCards && disabledCards.length === props.cardContext.length - 2) {
      setTimeout(() => emit("onFinish"), 1000)
    }
  } else if (rules.value.length === 2 && rules.value[0].value !== rules.value[1].value) {
    setTimeout(() => {
      cards[rules.value[0].index].onFlipBack()
      cards[rules.value[1].index].onFlipBack()
      rules.value = []

    }, 800)

  } else { return false }
}
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
  background-color: var(--dark);
  color: var(--light)
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
