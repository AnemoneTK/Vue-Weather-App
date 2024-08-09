<script setup>
import Weather from "./components/Weather.vue";
import SearchBar from "./components/SearchBar.vue";
const apiKey = import.meta.env.VITE_OPENWEATHER_API_KEY;
</script>

<template>
  <div class="h-svh bg-background flex items-center justify-center">
    <div
      class="card xs:p-5 md:p-2 xs:w-svw sm:w-2/3 md:w-1/3 h-5/6 rounded-xl bg-white drop-shadow-xl"
    >
      <SearchBar />
      <Weather :apiKey="apiKey" />
    </div>
  </div>
</template>

<style scoped></style>
