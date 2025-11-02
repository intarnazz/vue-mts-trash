<script setup></script>

<template>
  <div class="wrapper">
    <slot> </slot>
  </div>
</template>

<style lang="sass" scoped>
.wrapper
  width: 95dvw
  margin: 0 auto
  padding: 1rem 0

// @media screen and (max-width: 1440px)
//   .special-wrapper
//     width: 100%
//   .wrapper
//     width: 100%
//     margin: 0
//     padding: 0

// @media screen and (max-width: 590px)
//   .wrapper
//     width: 100%
//     margin: 0
//     padding: 0
</style>
