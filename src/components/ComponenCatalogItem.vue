<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps(['item'])
const emit = defineEmits(['choice_game'])
const avgColor = ref('black') // начальный цвет

async function color() {
  return new Promise((resolve, reject) => {
    const img = new Image()
    img.src = `data:image/jpeg;base64,${props.item.image_binary}`
    img.crossOrigin = 'Anonymous'
    img.onload = () => {
      const canvas = document.createElement('canvas')
      canvas.width = img.width
      canvas.height = img.height
      const ctx = canvas.getContext('2d')
      ctx.drawImage(img, 0, 0)

      const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height)
      const data = imageData.data

      let sumH = 0,
        sumS = 0,
        sumL = 0,
        count = 0

      for (let i = 0; i < data.length; i += 4) {
        const r = data[i] / 255
        const g = data[i + 1] / 255
        const b = data[i + 2] / 255

        const max = Math.max(r, g, b)
        const min = Math.min(r, g, b)
        let h,
          s,
          l = (max + min) / 2

        if (max === min) {
          h = s = 0
        } else {
          const d = max - min
          s = l > 0.5 ? d / (2 - max - min) : d / (max + min)
          switch (max) {
            case r:
              h = (g - b) / d + (g < b ? 6 : 0)
              break
            case g:
              h = (b - r) / d + 2
              break
            case b:
              h = (r - g) / d + 4
              break
          }
          h /= 6
        }

        h = h * 360
        s = s * 100
        l = l * 100

        // учитываем только яркие и насыщенные пиксели
        if (s >= 40 && l >= 40 && s <= 80 && l <= 80) {
          sumH += h
          sumS += s
          sumL += l
          count++
        }
      }

      if (count === 0) {
        // если нет ярких пикселей, берём серый как fallback
        resolve('hsl(0, 0%, 50%)')
      } else {
        let avgH = Math.round(sumH / count)
        const avgS = Math.round(sumS / count)
        const avgL = Math.round(sumL / count)
        if (avgH <= 240) avgH = avgH + 120
        else avgH = avgH - 120
        resolve(`hsl(${avgH}, ${avgS}%, ${avgL}%)`)
      }
    }

    img.onerror = reject
  })
}

// При монтировании компонента вычисляем цвет
onMounted(async () => {
  avgColor.value = await color()
})
</script>

<template>
  <div @click="emit('choice_game', item)" class="item box-y gap-[1rem] pr">
    <div class="pr">
      <div class="wh rounded-[10px] overflow-hidden">
        <img
          class="img baner pr z-[2]"
          :src="`data:image/jpeg;base64,${item.image_binary}`"
          alt=""
        />
      </div>
      <div class="pa z-2">
        <!-- <div
          :style="{ 'background-color': avgColor }"
          class="absolute w-[258px] right-0 bottom-[29px] bb px-[4px]"
        >
          <h3>{{ props.item.name }}</h3>
        </div> -->
        <div class="item__play-batton absolute right-[13px]">
          <img src="@/assets/icons/play.svg" alt="" />
        </div>
      </div>
    </div>
    <div class="box-x">
      <h3>{{ props.item.name }}</h3>
    </div>
  </div>
</template>

<style scoped lang="sass">
.item
  // margin-right: 2rem
  // margin-top: 2rem
  border-radius: 10px
  &__play-batton
    transition: .3s
    opacity: 0%
    box-shadow: 0 10px 10px 0 #0000009d
    border-radius: 100px
    bottom: 20px
  &:hover
    cursor: pointer
    outline: 1rem solid #2B2B2B
    background-color: #2B2B2B
    & .img.baner
      transform: scale(1.09)
    & .item
      margin-right: 2rem
      &__play-batton
        opacity: 100%
        bottom: 26px

  & .img
    transition: .15s
    height: 280px
    width: 280px
    border-radius: 10px
</style>
