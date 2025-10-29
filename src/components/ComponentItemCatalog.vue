<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps(['item'])
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
        const avgH = Math.round(sumH / count)
        const avgS = Math.round(sumS / count)
        const avgL = Math.round(sumL / count)
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
  <div class="item box-y gap-[0rem] pr">
    <div class="flex pr z-2">
      <p :style="{ color: avgColor }" >{{ item.name }}</p>
    </div>
    <div class="pr">
      <div
        :style="{ 'box-shadow': `0 0 180px -60px ${avgColor}` }"
        class="wh rounded-[10px] overflow-hidden"
      >
        <img
          class="img baner pr z-[2]"
          :src="`data:image/jpeg;base64,${item.image_binary}`"
          alt=""
        />
      </div>
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
</template>

<style scoped lang="sass">
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
</style>
