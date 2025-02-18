<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';
import axios from 'axios';

// Reactive array for genres
const genres = ref(['Amapiano', 'Pop', 'HipHop', 'Jazz']);

// Initialize router and toast
const router = useRouter();
const toast = useToast();

// Base API URL from environment variables
const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;

// Function to handle genre selection
const selectGenre = async (genre) => {
  try {
    // Make API call to update music preferences
    const response = await axios.put(`${apiBaseUrl}/music`, {
      playMusic: true,
      selectedGenre: genre.toLowerCase(),
    });

    // Handle successful response
    if (response.status === 200) {
      router.push('/friends'); // Navigate to the "friends" route
      toast.success('Music Activated!');
    }
  } catch (error) {
    // Handle errors
    console.error('Error updating music preferences:', error);
    toast.error('Music Activation Failed!');
  }
};
</script>

<template>
  <div class="background-image p-4 h-full flex flex-col items-center justify-center">
    <h3 class="text-white text-2xl mb-4 mt-8 text-center">
      Before we begin... Would you like some Music while you order your drinks?
    </h3>
    <h2 class="text-white text-4xl mb-8 text-center">Select a Genre</h2>
    <div class="flex flex-col space-y-8">
      <!-- Buttons for each genre -->
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