<template>
  <div class="screen">
    <div class="screen__inner" :style="{
      width: `${(((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3 / 4) + 16) * Math.sqrt(cardContext.length))}px`
      // ((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3 / 4 + margin right 1rem * item rows length
    
    }">
      <cart-item v-for="(card, index) in cardContext" :key="index" :ref="`card-${index}`"
        :cardUrl="`images/${card}.png`" :card="{ index, value: card }" @onFlip="checkRules" :rules="rules"
        :cardContext="cardContext" />
    </div>
  </div>
</template>

<script>
import CartItem from './CartItem.vue';
export default {
  props: {
    cardContext: {
      type: Array,
      default: function () {
        return []
      },
    },
  },
  data() {
    return {
      rules: [],
    }
  },
  methods: {
    checkRules(card) {
      if (this.rules.length === 2) {
        return false;
      } else {
        this.rules.push(card)
      }
      if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
        this.$refs[`card-${this.rules[0].index}`][0].onDisableCard()
        this.$refs[`card-${this.rules[1].index}`][0].onDisableCard()
        this.rules = []
        const disabledCards = document.querySelectorAll('.screen .card.disabled')
        if (disabledCards && disabledCards.length === this.cardContext.length - 2) {
          setTimeout(() => this.$emit("onFinish"), 1000)
        }
      } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBack()
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBack()
          this.rules = []

        }, 800)

      } else { return false }
    }
  },
  components: {
    CartItem
  }
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
