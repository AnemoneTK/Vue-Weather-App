<script setup>
import Weather from "./components/Weather.vue";
const apiKey = import.meta.env.VITE_OPENWEATHER_API_KEY;
</script>

<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <Weather :apiKey="apiKey" />
  </div>
</template>

<style>
/* Add global styles here */
</style>
