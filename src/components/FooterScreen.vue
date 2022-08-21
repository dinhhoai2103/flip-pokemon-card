<template>
  <div class="copyright">
    <button @click="onStartAgain">Back to menu</button>
  </div>
</template>
<script>
export default {
  methods: {
    onStartAgain() {
      this.$emit('onStartAgain')
    }
  }
}
</script>
 <style lang="css" scoped>
 .copyright {
   position: fixed;
   left: 50%;
   transform: translateX(-50%);
   bottom: 5px;
   color: var(--light);
   z-index: 3;
   font-size: 1.5rem;
 }
 
 .copyright button {
   font: var(--font);
   background: transparent;
   box-shadow: none;
   border: 1px solid var(--light);
   color: var(--light);
   padding: 1rem 1.25rem;
   border-radius: 0.5rem;
   font-size: 1.25rem;
   cursor: pointer;
   transition: background 0.3s ease-in-out;
 }
 </style>
