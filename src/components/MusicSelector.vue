<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';
import axios from 'axios';

const genres = ref(['Amapiano', 'Pop', 'HipHop', 'Jazz']);

const router = useRouter();

const toast = useToast();

const selectGenre = async (genre) => {
  
  try {
    const response = await axios.put('/api/music', {
      playMusic: true,
      selectedGenre: genre.toLowerCase(),
    });
    router.push('/friends');
    toast.success('Music Activated!');
  } catch (error) {
    console.error('Error updating music preferences:', error);
    toast.error('Music Activation Failed!');
  }
};
</script>

<template>
  <div class="background-image p-4 h-full flex flex-col items-center justify-center">
    <h3 class="text-white text-2xl mb-4 mt-8 text-center">Before we begin... Would you like some Music while you order your drinks?</h3>
    <h2 class="text-white text-4xl mb-8 text-center">Select a Genre</h2>
    <div class="flex flex-col space-y-8">
      <button
        v-for="genre in genres"
        :key="genre"
        @click="selectGenre(genre)"
        class="bg-orange-800 hover:bg-orange-700 text-white font-bold py-4 px-8 rounded"
      >
        {{ genre }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.background-image {
  background-image: url('@/assets/images/musicSelector.jpg');
  background-size: cover;
  background-position: center;
}
</style>
