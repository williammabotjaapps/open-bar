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
      <h2 class="text-white text-2xl mb-4">Select a Genre</h2>
      <div class="flex flex-col space-y-2">
        <button
          v-for="genre in genres"
          :key="genre"
          @click="selectGenre(genre)"
          class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        >
          {{ genre }}
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
