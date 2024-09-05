<template>
    <header class="bg-gray-800 text-white p-4 flex items-center justify-between">
      <h1 class="text-xl font-bold">My Music Player</h1>
      <audio ref="audioPlayer" :src="audioSrc" controls class="ml-4">
        Your browser does not support the audio element.
      </audio>
    </header>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const audioSrc = ref(require('@/assets/music/amapiano.mp3')); // Path to your MP3 file
  </script>
  
  <style scoped>
  /* No additional styles needed for this component */
  </style>
  