<script setup>
import {
  onUpdated,
  onBeforeUpdate,
  onBeforeUnmount,
  onBeforeMount,
  onMounted,
  onUnmounted,
  ref,
} from 'vue'
const timer = ref(0)
let intervalTimer = null

onMounted(() => {
  console.log('on mounted')
  intervalTimer = setInterval(() => {
    timer.value++
  }, 1000)
})

onBeforeMount(() => {
  console.log('on before mount')
})

onBeforeUpdate(() => {
  console.log('on before update')
})

onUpdated(() => {
  console.log('on updated')
})

onBeforeUnmount(() => {
  console.log('on before unmount')
})

onUnmounted(() => {
  console.log('on unmounted')
  if (intervalTimer) {
    clearInterval(intervalTimer)
  }
})
</script>

<template>
  <div class="about">
    <h1>Time: {{ timer }}</h1>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
