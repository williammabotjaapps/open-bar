<script setup>

const dontPlayMusic = () => {
  console.log("User chose not to play music.");
};
</script>

<template>
  <div class="flex h-screen">
    <div class="w-1/2 bg-gray-900">
    </div>
    <div class="w-1/2 bg-gray-800 p-4 flex items-center justify-center">
      <h3>No, Thanks</h3>
      <button
        @click="dontPlayMusic"
        class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
      >
        Don't Play Music
      </button>
    </div>
  </div>
</template>

<style scoped>

</style>
