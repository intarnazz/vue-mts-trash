<script setup>
import LayoutPage from '@/layout/LayoutPage.vue'
import SectionCatalog from '@/components/sections/SectionCatalog.vue'
import SectionStatus from '@/components/sections/SectionStatus.vue'
import SectionGame from '@/components/sections/SectionGame.vue'
import LayoutWrapper from '@/layout/LayoutWrapper.vue'
import LayoutBG from '@/layout/LayoutBG.vue'
import { ref } from 'vue'

const choice_game = ref(null)
</script>

<template>
  <LayoutPage class="main home">
      <LayoutWrapper class="wrapper">
        <div class="box-x">
          <div class="flex"></div>
          <div class="flex">
            <div class="p-[2rem] box-y gap2">
              <!-- <SectionGame v-if="choice_game" :item="choice_game" /> -->
            </div>
          </div>
        </div>
        <SectionCatalog v-if="!choice_game" @choice_game="(i) => (choice_game = i)" />
      </LayoutWrapper>
  </LayoutPage>
</template>

<style lang="sass"></style>
