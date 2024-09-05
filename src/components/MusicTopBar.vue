
<script setup>
import { reactive, onMounted, onUnmounted } from 'vue';
import { Howl } from 'howler';

import audioSrc from '/music/amapiano.mp3';

const state = reactive({
  sound: null,
});

const initSound = () => {
  state.sound = new Howl({
    src: [audioSrc],
    html5: true, 
    onload: () => {
      state.sound.play();
    },
  });
};

const play = () => {
  if (!state.sound) {
    initSound();
  } else {
    state.sound.play();
  }
};

const pause = () => {
  if (state.sound) {
    state.sound.pause();
  }
};

const stop = () => {
  if (state.sound) {
    state.sound.stop();
  }
};

onMounted(() => {
  initSound();
});

onUnmounted(() => {
  if (state.sound) {
    state.sound.stop();
  }
});
</script>

<style scoped>

</style>


<template>
    <header class="bg-black text-white p-4 flex items-center justify-between">
      <h1 class="text-xl font-bold">My Music Player</h1>
      <div class="flex items-center">
        <button @click="play" class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded">
          Play
        </button>
        <button @click="pause" class="bg-yellow-500 hover:bg-yellow-600 text-white font-bold py-2 px-4 rounded ml-2">
          Pause
        </button>
        <button @click="stop" class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded ml-2">
          Stop
        </button>
      </div>
    </header>
  </template>
  