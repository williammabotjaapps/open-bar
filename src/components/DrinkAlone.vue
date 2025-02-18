<script setup>
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';
import axios from 'axios';

// Initialize router and toast
const router = useRouter();
const toast = useToast();

// Base API URL from environment variables
const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;

// Function to handle "Drink Alone" action
const drinkAlone = async () => {
  try {
    // Make API call to update friends preferences
    const response = await axios.put(`${apiBaseUrl}/friends`, {
      withFriends: false,
      numberOfFriends: 0,
    });

    // Handle successful response
    if (response.status === 200) {
      router.push('/tab'); // Navigate to the "tab" route
      toast.success('Updated Friends Count!');
    }
  } catch (error) {
    // Handle errors
    console.error('Error updating friends preferences:', error);
    toast.error('Failed to Update Friends Count!');
  }
};
</script>

<template>
  <div class="background-image p-4 flex items-center justify-center h-full">
    <h3 class="text-white mr-4">No, Thanks</h3>
    <button
      @click="drinkAlone"
      class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
    >
      Drink Alone
    </button>
  </div>
</template>

<style scoped>
.background-image {
  background-image: url('@/assets/images/drinkAlone.jpg');
  background-size: cover;
  background-position: center;
}
</style>