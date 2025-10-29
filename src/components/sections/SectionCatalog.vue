<script setup>
import { computed, onMounted, ref } from 'vue'
import LayoutLoding from '@/layout/LayoutLoding.vue'

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
  <section class="clatalog" v-if="!isLoding">
    <!-- <h2>
      {{ catalog }}
    </h2> -->
    <div class="list">
      <div v-for="item in catalog" :key="item.id" class="item box-y gap-[0rem] pr">
        <div class="flex pr z-2 opacity-50">
          <h3>{{ item.name }}</h3>
        </div>
        <img
          class="img blur-[100px] pa scale-150 z-[1]"
          :src="`data:image/jpeg;base64,${item.image_binary}`"
          alt=""
        />
        <div class="wh overflow-hidden">
          <img
            class="img baner pr z-[2]"
            :src="`data:image/jpeg;base64,${item.image_binary}`"
            alt=""
          />
        </div>
        <div class="pa p-[.5rem] z-2 box-y">
          <div class="flex"></div>
          <div class="box-x gap">
            <div class="flex"></div>
            <div class="">
              <img src="@/assets/icons/play.svg" alt="" />
            </div>
          </div>
        </div>
        <div class="flex"></div>
      </div>
    </div>
  </section>
</template>

<style lang="sass">
.item
  &:hover
    cursor: pointer
    & .img.baner
      transform: scale(1.1)
      
  & .img
    transition: .15s
    width: 312px
    height: 312px
    border-radius: 10px

.list
  display: grid
  grid-template-columns: 1fr 1fr 1fr 1fr
  gap: 1rem
</style>
