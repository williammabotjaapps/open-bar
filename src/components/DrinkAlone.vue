<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
  <div class="background-image p-4 flex items-center justify-center h-full">
    <h3 class="text-white mr-4">No, Thanks</h3>
    <RouterLink
      to="/tab"
      class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
    >
      Don't Play Music
    </RouterLink>
  </div>
</template>

<style scoped>
.background-image {
  background-image: url('@/assets/images/noMusic.jpg');
  background-size: cover;
  background-position: center;
}
</style>
