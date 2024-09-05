<script setup>
import MusicSelector from '@/components/MusicSelector.vue';
import NoMusic from '@/components/NoMusic.vue';
</script>

<template>
  <div class="flex h-screen">

    <div class="w-1/2">
      <MusicSelector />
    </div>
    <div class="w-1/2">
      <NoMusic />
    </div>
  </div>
</template>

