<script setup>
import { onMounted, ref } from 'vue'
import ComponentItemCatalog from '@/components/ComponenCatalogItem.vue'

const props = defineProps(['item'])
const isLoding = ref(true)

function sendToCppAsync(json) {
  return new Promise((resolve, reject) => {
    if (!window.sendToCpp) {
      reject(new Error('sendToCpp ещё не инициализирован'))
      return
    }
    window.sendToCpp(json, (error, result) => {
      if (error) {
        reject(new Error(error))
      } else {
        resolve(result)
      }
    })
  })
}

async function init() {
  isLoding.value = true
  try {
    const res = await sendToCppAsync(
      JSON.stringify({
        path: {
          name: 'game',
          type: 'content',
          content: props.item.path,
        },
      }),
    )
    c.value = JSON.parse(res)
  } catch (error) {
    console.error('Ошибка в sendToCpp:', error)
    catalog.value = `Ошибка: ${error.message}`
  }
  isLoding.value = false
}

onMounted(() => {
  // init()
})
</script>

<template>
  <aside
    :class="{
      'w-[0px] right-[-100px]': !props.item,
    }"
    class="game box pr right-0"
  >
    <ComponentItemCatalog v-if="props.item" class="pointer-events-none" :item="props.item" />
  </aside>
</template>

<style scoped lang="sass">
.game
  transition: .3s
</style>
