<script setup>
import LayoutPage from '@/layout/LayoutPage.vue'
import SectionCatalog from '@/components/sections/SectionCatalog.vue'
import SectionStatus from '@/components/sections/SectionStatus.vue'
import LayoutWrapper from '@/layout/LayoutWrapper.vue'
import LayoutBG from '@/layout/LayoutBG.vue'
</script>

<template>
  <LayoutPage class="main home">
    <LayoutBG>
      <LayoutWrapper class="wrapper">
        <div class="box-x">
          <div class="flex"></div>
          <div class="flex">
            <div class="p-[2rem]">
              <SectionStatus class="pr z-[2]" />
            </div>
          </div>
        </div>
        <SectionCatalog />
      </LayoutWrapper>
    </LayoutBG>
  </LayoutPage>
</template>

<style lang="sass"></style>
