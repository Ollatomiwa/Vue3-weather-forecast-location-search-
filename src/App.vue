<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
    <NavigationBar />
    <RouterView />
  </div>
</template>

<script setup>
import {RouterView} from 'vue-router';
import NavigationBar from './components/NavigationBar.vue';
</script>

<style lang="scss" scoped>

</style>