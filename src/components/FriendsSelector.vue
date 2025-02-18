<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';
import axios from 'axios';

// Reactive variable for the number of friends
const friendsCount = ref(0);

// Initialize router and toast
const router = useRouter();
const toast = useToast();

// Base API URL from environment variables
const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;

// Function to handle "Continue with Friends" action
const continueWithFriends = async () => {
  try {
    // Make API call to update friends preferences
    const response = await axios.put(`${apiBaseUrl}/friends`, {
      withFriends: true,
      numberOfFriends: friendsCount.value,
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
  <div class="background-image p-4 h-full flex flex-col items-center justify-center">
    <h3 class="text-white text-2xl mb-4 mt-8 text-center">
      One More Question... Are you drinking with friends or drinking alone?
    </h3>
    <h2 class="text-white text-4xl mb-8 text-center">Specify the number of friends</h2>
    <div class="flex flex-col items-center space-y-8">
      <!-- Input for number of friends -->
      <input
        type="number"
        v-model="friendsCount"
        class="text-center text-4xl border border-gray-300 rounded p-2 w-full max-w-xs md:max-w-md lg:max-w-lg"
        min="1"
        placeholder="1"
      />

      <!-- Button to continue -->
      <button
        @click="continueWithFriends"
        class="bg-blue-600 hover:bg-blue-500 text-white font-bold py-4 px-8 rounded"
      >
        Continue
      </button>
    </div>
  </div>
</template>

<style scoped>
.background-image {
  background-image: url('@/assets/images/friendsSelector.jpg');
  background-size: cover;
  background-position: center;
}
</style>