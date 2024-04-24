<script setup>
import DrawArea from '@/components/DrawArea.vue'
</script>

<template>
  <main>
    <span>Draw a number</span>
    <span>and give the ML model a try to guess it</span>
    <DrawArea />
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  place-items: center;
  place-content: center;
}
span {
  text-align: center;
  color: rgb(31, 34, 37);
  font-weight: 500;
  font-size: 1.5rem;
}
</style>
