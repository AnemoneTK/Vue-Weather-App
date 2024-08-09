<script setup>
import { ref } from "vue";

//props
const props = defineProps({
  apiKey: {
    type: String,
    required: true,
  },
});

const city = ref("");
const weather = ref(null);

const getWeather = async () => {
  const response = await fetch(
    `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=${props.apiKey}`
  );
  const data = await response.json();
  weather.value = data;
};
</script>
<template>
  <div class="max-w-md mx-auto mt-10 p-4 bg-white shadow-md rounded-lg">
    <input
      type="text"
      v-model="city"
      placeholder="Enter city"
      class="border p-2 rounded w-full"
    />
    <button
      @click="getWeather"
      class="bg-blue-500 text-white p-2 rounded mt-2 w-full"
    >
      Get Weather
    </button>
    <div v-if="weather">
      <h2 class="text-2xl font-bold mt-4">{{ weather.name }}</h2>
      <p class="text-lg">{{ weather.weather[0].description }}</p>
      <p class="text-lg">Temp: {{ weather.main.temp }} °C</p>
      <p class="text-lg">Humidity: {{ weather.main.humidity }}%</p>
    </div>
  </div>
</template>

<style scoped>
/* Add any scoped styles here */
</style>
