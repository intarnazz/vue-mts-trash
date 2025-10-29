<script setup>
const props = defineProps(['fill'])
</script>

<template>
  <svg width="56" height="56" viewBox="0 0 56 56" fill="none" xmlns="http://www.w3.org/2000/svg">
    <rect class="opacity-70" width="56" height="56" rx="28" :fill="props.fill" />
    <path
      class="opacity-100"
      d="M19.5056 20.891C19.5056 18.5895 21.9902 17.1452 23.9901 18.284L37.5225 25.99C39.5555 27.1476 39.54 30.0833 37.495 31.2194L23.9625 38.7374C21.9629 39.8483 19.5056 38.4024 19.5056 36.115V20.891Z"
      fill="black"
    />
  </svg>
</template>

<style scoped lang="sass"></style>
