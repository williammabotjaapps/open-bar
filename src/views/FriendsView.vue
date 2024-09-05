<script setup>
import FriendsSelector from '@/components/FriendsSelector.vue';
import DrinkAlone from '@/components/DrinkAlone.vue';
import MusicTopBar from '@/components/MusicTopBar.vue';
</script>

<template>
  <MusicTopBar />
  <div class="flex h-screen">
    <div class="w-1/2">
      <FriendsSelector />
    </div>
    <div class="w-1/2">
      <DrinkAlone />
    </div>
  </div>
</template>

