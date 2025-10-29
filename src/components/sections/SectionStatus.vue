<script setup>
import { ref } from 'vue'

const mts_status = ref(NaN)
const api_status = ref(NaN)
const errors = ref([])
</script>

<template>
  <section class="status">
    <h3>MTS STATUS: {{ mts_status }}</h3>
    <h3>API STATUS: {{ api_status }}</h3>
    <h3>ERRORS: {{ errors.length }}</h3>
  </section>
</template>

<style lang="sass"></style>
