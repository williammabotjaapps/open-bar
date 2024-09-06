<script setup>
  import PulseLoader from 'vue-spinner/src/PulseLoader.vue'; 
</script>

<template>
    <section class="bg-blue-50 px-4 py-10">
      <div class="container-xl lg:container m-auto">
        <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
          Loading...
        </h2>
        <div class="text-center text-gray-500 py-6">
          <PulseLoader />
        </div>
      </div>
    </section>
  </template>
  
  