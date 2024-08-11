<script setup>
import Weather from "./components/Weather.vue";
import SearchBar from "./components/SearchBar.vue";
import Setting from "./components/Setting.vue";
import countryList from "./assets/countryList.json";
import { ref } from "vue";

const apiKey = import.meta.env.VITE_OPENWEATHER_API_KEY;

const country = ref("");
const language = ref("en");
const units = ref("metric");
</script>

<template>
  <div
    class="h-svh bg-background flex items-center justify-center overflow-hidden"
  >
    <div
      class="card w-full h-full md:w-5/6 md:h-5/6 lg:w-3/6 lg:h-5/6 xl:w-1/3 xl:h-5/6 md:rounded-3xl bg-white px-3 md:px-5 lg:px-5 py-10 drop-shadow-xl md:border-2"
    >
      <div class="grid grid-cols-5 m-0">
        <div class="col-span-4 flex justify-start">
          <SearchBar :list="countryList" v-model="country" />
        </div>
        <div class="flex justify-end">
          <Setting v-model:language="language" v-model:units="units" />
        </div>
      </div>
      <div class="w-full h-72 p-0">
        <Weather
          :apiKey="apiKey"
          :country="country"
          :language="language"
          :units="units"
        />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
