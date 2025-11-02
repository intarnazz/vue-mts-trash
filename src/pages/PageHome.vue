<script setup>
import LayoutPage from '@/layout/LayoutPage.vue'
import SectionCatalog from '@/components/sections/SectionCatalog.vue'
// import SectionStatus from '@/components/sections/SectionStatus.vue'
import SectionGame from '@/components/sections/SectionGame.vue'
import LayoutWrapper from '@/layout/LayoutWrapper.vue'
// import LayoutBG from '@/layout/LayoutBG.vue'
import { ref } from 'vue'

const choice_game = ref(null)
</script>

<template>
  <LayoutPage class="main home">
    <LayoutWrapper class="wrapper box-x as gap-[1rem]">
      <SectionCatalog
        class="flex"
        @choice_game="(game) => (choice_game = game)"
      />
      <SectionGame :item="choice_game" />
    </LayoutWrapper>
  </LayoutPage>
</template>

<style scoped lang="sass"></style>
