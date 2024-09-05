<script setup>
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  import { useToast } from 'vue-toastification';
  import axios from 'axios';

  const friendsCount = ref(0);
  const toast = useToast();

  const router = useRouter();

  const continueWithFriends = async () => {
  try {
        const response = await axios.put('/api/friends', {
        withFriends: true,
        numberOfFriends: friendsCount.value,
        });
        if (response.status == 200)
        {
            router.push('/tab');
            toast.success('Updated Friends Count!');
        }
        } catch (error) {
            console.error('Error updating friends preferences:', error);
            toast.error('Failed to Update Friends Count!');
        }
    };

  </script>

<template>
    <div class="background-image p-4 h-full flex flex-col items-center justify-center">
      <h3 class="text-white text-2xl mb-4 mt-8 text-center">One More Question... Are you drinking with friends or drinking alone?</h3>
      <h2 class="text-white text-4xl mb-8 text-center">Specify the number of friends</h2>
      <div class="flex flex-col items-center space-y-8">
        <input
            type="number"
            v-model="friendsCount"
            class="text-center text-4xl border border-gray-300 rounded p-2 w-full max-w-xs md:max-w-md lg:max-w-lg"
            min="1"
            placeholder="1"
        />

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
  