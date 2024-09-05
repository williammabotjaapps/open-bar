<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
  <section class="background-image text-center flex flex-col justify-center items-center h-screen">
    <i class="pi pi-exclamation-triangle text-yellow-500 text-7xl mb-5"></i>
    <h1 class="text-6xl font-bold mb-4 text-white">404 Not Found</h1>
    <p class="text-xl mb-5 text-white">This bar is still open though</p>
    <RouterLink
      to="/"
      class="text-white bg-orange-800 hover:bg-orange-700 rounded-md px-3 py-2 mt-4"
      >Go Back</RouterLink
    >
  </section>
</template>

<style scoped>
.background-image {
  background-image: url('@/assets/images/notFound.jpg');
  background-size: cover;
  background-position: center;
}
</style>