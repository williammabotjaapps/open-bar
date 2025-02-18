<script setup>
import axios from 'axios';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';

// Initialize toast and router
const toast = useToast();
const router = useRouter();

// Base API URL from environment variables
const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;

// Function to deactivate music
const deactivateMusic = async () => {
  try {
    // Make API call to update music preferences
    const response = await axios.put(`${apiBaseUrl}/music`, {
      playMusic: false,
      selectedGenre: '',
    });

    // Handle successful response
    if (response.status === 200) {
      router.push('/friends'); // Navigate to the "friends" route
      toast.success('Music Deactivated!');
    }
  } catch (error) {
    // Handle errors
    console.error('Error updating music preferences:', error);
    toast.error('Music Deactivation Failed!');
  }
};
</script>

<template>
  <div class="background-image p-4 flex items-center justify-center h-full">
    <h3 class="text-white mr-4">No, Thanks</h3>
    <button
      @click="deactivateMusic"
      class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
    >
      Don't Play Music
    </button>
  </div>
</template>

<style scoped>
.background-image {
  background-image: url('@/assets/images/noMusic.jpg');
  background-size: cover;
  background-position: center;
}
</style>