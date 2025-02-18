<template>
  <header v-if="state.playMusic" class="bg-black text-white p-4 flex items-center justify-between">
    <h1 class="text-xl font-bold">OpenBar Player</h1>
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

<script setup>
import { reactive, onMounted, onUnmounted } from 'vue';
import { Howl } from 'howler';
import axios from 'axios';

const state = reactive({
  sound: null,
  playMusic: false,
  selectedGenre: '',
});

const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;

const initSound = () => {
  if (state.playMusic) {
    const audioSrc = `{apiBaseUrl}/music/${state.selectedGenre}.mp3`;
    state.sound = new Howl({
      src: [audioSrc],
      html5: true,
      onload: () => {
        state.sound.play();
      },
    });
  }
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

onMounted(async () => {
  try {

    // Fetch music preferences
    const response = await axios.get(`${apiBaseUrl}/music/`);
    state.playMusic = response.data.playMusic;
    state.selectedGenre = response.data.selectedGenre;
    initSound();
  } catch (error) {
    console.error('Error fetching music preferences:', error);
  }
});

onUnmounted(() => {
  if (state.sound) {
    state.sound.stop();
  }
});
</script>

<style scoped>

</style>