<script setup>
import Weather from "./components/Weather.vue";
import SearchBar from "./components/SearchBar.vue";
import countryList from "./assets/countryList.json";
import { ref } from "vue";

const apiKey = import.meta.env.VITE_OPENWEATHER_API_KEY;

const country = ref("");
</script>

<template>
  <div class="h-svh bg-background flex items-center justify-center">
    <div
      class="card xs:w-full xs:h-full sm:w-full sm:h-full md:w-5/6 md:h-5/6 lg:w-1/3 lg:h-5/6 xl:w-1/3 rounded-3xl bg-white px-5 py-10 drop-shadow-xl border-2"
    >
      <SearchBar :list="countryList" v-model="country" />
      <Weather :apiKey="apiKey" :country="country" />
    </div>
  </div>
</template>

<style scoped></style>
