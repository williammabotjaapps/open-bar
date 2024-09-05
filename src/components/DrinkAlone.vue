<script setup>
import { RouterLink } from 'vue-router';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';
import axios from 'axios';

const router = useRouter();
const toast = useToast();

const drinkAlone = async () => {
  try {
        const response = await axios.put('/api/friends', {
        withFriends: false,
        numberOfFriends: 0,
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
