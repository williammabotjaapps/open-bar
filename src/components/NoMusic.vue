<script setup>
import axios from 'axios';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';

const toast = useToast();

const router = useRouter();

const deactivateMusic = async () => {
  
  try {
    const response = await axios.put('/api/music', {
      playMusic: false,
      selectedGenre: '',
    });
    if (response.status == 200)
      router.push('/friends');
      toast.success('Music Deactivated!');
  } catch (error) {
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
