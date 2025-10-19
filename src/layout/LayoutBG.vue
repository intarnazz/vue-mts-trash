<script setup>
import { onMounted, ref } from 'vue'

const token = ref(null)

onMounted(() => {
  token.value = sessionStorage.getItem('token')
})
</script>

<template>
  <div class="bg pa box-x">
    <div class="flex h texture"></div>
    <div class="flex h">
      <img class="img" src="@/assets/img/bg-ascii-fece.png" alt="" />
    </div>
  </div>
  <div class="pr">
    <slot> </slot>
  </div>
</template>

<style lang="sass" scoped>
.texture
  background-image: url('@/assets/img/texture.png');
  opacity: .1
  background-size: 7% 7%;

  animation: moveTexture 30s linear infinite alternate;
// .main
//   margin-top: $header
.main
  justify-content: flex-start
  align-items: center

@keyframes moveTexture
  0%
    background-position: 0 0;

  100%
    background-position: -100% 100%;
</style>
