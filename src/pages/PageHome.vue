<script setup>
import LayoutPage from '@/layout/LayoutPage.vue'
import SectionCatalog from '@/components/sections/SectionCatalog.vue'
import LayoutWrapper from '@/layout/LayoutWrapper.vue'
import LayoutBG from '@/layout/LayoutBG.vue'
</script>

<template>
  <LayoutPage class="main home">
    <LayoutBG>
      <LayoutWrapper class="wrapper">
        <SectionCatalog />
      </LayoutWrapper>
    </LayoutBG>
  </LayoutPage>
</template>

<style lang="sass"></style>
