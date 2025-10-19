<script setup>
import { computed, onMounted, ref } from 'vue'
import LayoutLoding from '@/layout/LayoutLoding.vue'

const c = ref('')
const isLoding = ref(true)

const catalog = computed(() => {
  return c.value
})

function init() {
  if (window.sendToCpp) {
    const res = window.sendToCpp(
      JSON.stringify({
        path: {
          name: 'game',
          type: 'get',
        },
      }),
    )
    c.value = JSON.parse(res)
  } else {
    catalog.value = 'sendToCpp ещё не инициализирован'
  }
  isLoding.value = false
}

onMounted(() => {
  init()
})
</script>

<template>
  <LayoutLoding v-if="isLoding" />
  <template v-if="!isLoding">
    <!-- <h2>
      {{ catalog }}
    </h2> -->
    <div class="list">
      <div v-for="item in catalog" :key="item.id" class="item">
        <img class="img" :src="item.url" alt="" />
      </div>
    </div>
  </template>
</template>

<style lang="sass">
.item
  & .img
    width: 312px
    height: 312px
    border-radius: 10px
.list
  display: grid
  grid-template-columns: 1fr 1fr 1fr 1fr
  gap: 1rem
</style>
