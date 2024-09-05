<script setup>
import { ref } from 'vue';

const genres = ref(['Amapiano', 'Pop', 'Hip Hop', 'Jazz']);

const selectGenre = (genre) => {
  console.log(`Selected genre: ${genre}`);
}
</script>

<template>
  <div class="flex h-screen">
    <div class="w-1/2 bg-gray-800 p-4">
      <h3 class="text-white text-2xl mb-4 mt-8 text-center">Before we begin... Would you like some Music while you order your drinks</h3>
      <h2 class="text-white text-4xl mb-8 text-center">Select a Genre</h2>
      <div class="flex flex-col space-y-8">
        <button
          v-for="genre in genres"
          :key="genre"
          @click="selectGenre(genre)"
          class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-4 px-8 rounded"
        >
          {{ genre }}
        </button>
      </div>
    </div>
  </div>
</template>

<style>
.background-image {
  background-image: url('../assets/images/drinksWelcome.jpg');
  background-size: cover;
  background-position: center;
}
</style>
