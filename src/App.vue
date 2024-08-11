<script setup>
import Weather from "./components/Weather.vue";
import SearchBar from "./components/SearchBar.vue";
import Setting from "./components/Setting.vue";
import countryList from "./assets/countryList.json";
import { ref } from "vue";

const apiKey = import.meta.env.VITE_OPENWEATHER_API_KEY;

const country = ref("");
</script>

<template>
  <div class="h-svh bg-background flex items-center justify-center">
    <div
      class="card xs:w-full xs:h-full sm:w-full sm:h-full md:w-5/6 md:h-5/6 lg:w-1/3 lg:h-5/6 xl:w-1/3 rounded-3xl bg-white sm:px-2 md:px-2 py-10 drop-shadow-xl border-2"
    >
      <div class="flex flex-row align-middle justify-start p-0 m-0">
        <div class="w-10/12">
          <SearchBar :list="countryList" v-model="country" />
        </div>
        <Setting />
      </div>
      <Weather :apiKey="apiKey" :country="country" />
    </div>
  </div>
</template>

<style scoped></style>
