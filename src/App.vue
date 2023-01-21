<template>
  <div class="flex flex-col min-h-screen font-Kanit bg-primary-color">
    <NavBar />
    <RouterView />
  </div>
</template>

<script setup>
import {RouterView} from "vue-router";
import NavBar from "./components/NavBar.vue";
</script>

<style lang="scss" scoped>

</style>