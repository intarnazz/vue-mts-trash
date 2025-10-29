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
  position: relative
  overflow: hidden

.texture::before
  content: ''
  position: absolute
  top: -100%
  left: 0
  width: 200%
  height: 200%
  background-image: url('@/assets/img/texture.png')
  background-size: 7% 7%
  opacity: 0.1
  animation: moveTexture 2s linear infinite 
// .main
//   margin-top: $header
.main
  justify-content: flex-start
  align-items: center

@keyframes moveTexture
  0%
    transform: translate(0, 0)
  100%
    transform: translate(-6.95%, 6.95%)
</style>
