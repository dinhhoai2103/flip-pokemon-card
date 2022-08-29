<template>
  <div class="card" :class="{ disabled: isDisabled }" :style="{
    height: `${(920 - 16 * 4) / Math.sqrt(cardContext.length) - 16}px`,
    // window height 920px - 16*4 (margin t-b 2rem) / height column - 16 (mb 1rem)
    width: `${(((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4}px`, //width = 3/4 height
    perspective: `${(((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 * 2}px`
  }">
    <div class="card__inner" :class="{ 'is-flipped': isFlipped }" @click="onToggleFlipCard">
      <div class="card__face card__face--front">
        <div class="card-content"></div>
      </div>
      <div class="card__face card__face--back">
        <div class="card-content" :style="{ backgroundImage: `url(${require('@/assets/' + cardUrl)}) ` }"></div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue'
const isFlipped = ref(false)
const isDisabled = ref(false)
const props = defineProps({
  card: {
    type: [String, Number, Array, Object],
    required: true
  },
  cardUrl: {
    type: String,
    required: true
  },
  rules: {
    type: Array,
    default: function () {
      return []
    }
  },
  cardContext: {
    type: Array,
    default: function () {
      return []
    }
  }
})
const emit = defineEmits(['onFlip'])
const onFlipBack = () => {
  isFlipped.value = false
}
const onDisableCard = () => {
  isDisabled.value = true
}
defineExpose({
  name: 'cards',
  onFlipBack,
  onDisableCard
})
const onToggleFlipCard = () => {
  if (isDisabled.value)
    return false;
  if (props.rules.length === 2)
    return false;
  isFlipped.value = !isFlipped.value
  if (isFlipped.value)
    emit('onFlip', props.card)
}


</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front {
  background-color: var(--dark);
}

.card__face--front .card-content {
  background: url('../assets/images/icon_back.png') no-repeat center center;
  background-size: contain;
  height: 100%;
  width: 100%;
}


.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card-content {
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center center;
  height: 100%;
  width: 100%;
}

.card.disabled .card__inner {
  cursor: default;
}
</style>
