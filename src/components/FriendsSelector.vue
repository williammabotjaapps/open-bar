<script setup>
  import { ref } from 'vue';
  
  // Create a reactive reference for the number of friends
  const friendCount = ref(0);
  
  // Function to increase the friend count by one
  const increaseCount = () => {
    friendCount.value += 1;
  };
  </script>

<template>
    <div class="background-image p-4 h-full flex flex-col items-center justify-center">
      <h3 class="text-white text-2xl mb-4 mt-8 text-center">One More Question... Are you drinking with friends or drinking alone?</h3>
      <h2 class="text-white text-4xl mb-8 text-center">Specify the number of friends</h2>
      <div class="flex flex-col items-center space-y-8">
        <input
          type="number"
          v-model="friendCount"
          class="text-center text-4xl border border-gray-300 rounded p-2"
          min="0"
          placeholder="0"
        />
        <button
          @click="increaseCount"
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
  