<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
  <div class="h-screen flex items-center justify-center relative">
    <div class="background-image absolute inset-0 opacity-75"></div>
    <div class="relative z-10 text-center text-white">
      <h1 class="text-2xl font-bold mb-4">Welcome to the OpenBar, Enjoy your time here.</h1>
      <RouterLink to="/music" class="bg-orange-800 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded">
        Get Started
      </RouterLink>
    </div>
  </div>
</template>

<style scoped>
.background-image {
  background-image: url('@/assets/images/drinksWelcome.jpg');
  background-size: cover;
  background-position: center;
}
</style>
