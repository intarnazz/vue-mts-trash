<script setup>
import { computed, onMounted, ref } from 'vue'
import LayoutLoding from '@/layout/LayoutLoding.vue'
import ComponentItemCatalog from '@/components/ComponenCatalogItem.vue'

const emit = defineEmits(['choice_game'])
const c = ref('')
const isLoding = ref(true)

const catalog = computed(() => {
  return c.value
})

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
          type: 'get',
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
  init()
})
</script>

<template>
  <LayoutLoding v-if="isLoding" />
  <section class="clatalog box box-y gap2" v-if="!isLoding">
    <!-- <h2>
      {{ catalog }}
    </h2> -->
    <h2>Самые новые</h2>
    <div class="box-x">
      <div class="list">
        <template v-for="item in catalog" :key="item.id">
          <ComponentItemCatalog @choice_game="(i) => emit('choice_game', i)" :item="item" />
        </template>
      </div>
    </div>
  </section>
</template>

<style scoped lang="sass">
.list
  display: grid
  grid-template-columns: 1fr 1fr 1fr 1fr
</style>
