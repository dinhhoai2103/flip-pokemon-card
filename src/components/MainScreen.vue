<template>
  <div class="screen">
    <h1>Pokemon Slip Card</h1>
    <h3>Please select mode to start game</h3>
    <div class="modes-container">
      <div class="modes" v-for="mode in modes" :key="mode.name">
        <button @click="$emit('onStart', { totalBlocks: mode.total })">
          <span>{{ mode.type }}</span>
          <span>{{ mode.name }}</span>
        </button>
      </div>
    </div>
  </div>
</template>
<script setup>
defineProps({
  modes: Array,
  default: function () {
    return []
  }
})
defineEmits(['onStart'])
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: var(--dark);
  color: var(--light);
}

.screen h1 {
  font-size: 4.5rem;
  text-transform: uppercase;
}

.screen h3 {
  font-size: 2rem;
}

.screen p {
  font-size: 2rem;
}

.modes-container {
  display: flex;
}

.modes {
  display: flex;
  margin-top: 2rem;
}

.modes button {
  font: var(--font);
  width: 150px;
  height: 150px;
  background: transparent;
  box-shadow: none;
  border: 1px solid var(--light);
  color: var(--light);
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 0 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}

.modes button:hover {
  background-color: var(--light);
  color: var(--dark);
}

.modes button span:first-child {
  font-size: 2rem;
}

.modes button span:last-child {
  display: block;
  font-size: 1.25rem;
  margin-top: 0.5rem;
}
</style>
