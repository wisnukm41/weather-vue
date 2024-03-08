<template>
 <div
    class="flex flex-col min-h-screen bg-weather-primary"
  >

    <SiteNavigation />
    <RouterView v-slot="{Component}">
      <Transition name="page" mode="out-in">
        <component :is="Component" />
      </Transition>
    </RouterView>
  </div>
</template>

<script setup>
  import { RouterView } from "vue-router"
  import SiteNavigation from "./components/SiteNavigation.vue"
</script>

<style>
  .page-enter-active,
  .page-leave-active {
    transition: all ease 600ms;
  }

  .page-enter-from,
  .page-leave-to {
    opacity: 0;
  }
</style>