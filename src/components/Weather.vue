<script setup>
import { ref, watch, onMounted } from "vue";
import Icon from "./Icon.vue";
import HumidityIcon from "../assets/weatherIcon/icon_humidity.png";
import WindSpeedIcon from "../assets/weatherIcon/icon_windSpeed.png";

// Props
const props = defineProps({
  apiKey: {
    type: String,
    required: true,
    default: "",
  },
  country: {
    type: String,
    required: false,
    default: "",
  },
});

const weather = ref(null);
const icon = ref("");
const hour = ref(null);
const date = ref("");
const time = ref("");

// Fetch weather data and time data
const getWeather = async () => {
  if (props.country) {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${props.country}&units=metric&appid=${props.apiKey}&lang=th`
      );
      if (!response.ok) {
        throw new Error("Failed to fetch weather data");
      }
      const data = await response.json();
      weather.value = data;

      icon.value = data.weather[0].icon;

      const lat = data.coord.lat;
      const lon = data.coord.lon;

      const timeResponse = await fetch(
        `http://localhost:3000/api/time?lat=${lat}&lon=${lon}`
      );
      if (!timeResponse.ok) {
        throw new Error("Failed to fetch time data");
      }
      const timeData = await timeResponse.json();
      hour.value = timeData.hour;
      date.value = timeData.date;
      time.value = timeData.time;
    } catch (error) {
      console.error(error.message);
    }
  }
};

watch(
  () => props.country,
  () => {
    getWeather();
  }
);

onMounted(() => {
  getWeather();
});
</script>

<template>
  <div class="sm:px-5 md:px-5 lg:px-5 pt-0 mt-5 text-primary">
    <div v-if="weather">
      <Icon :Icon="icon" :hour="hour" />
      <p class="text-lg text-center">{{ weather.weather[0].description }}</p>
      <p class="text-6xl my-4 text-center font-bold">
        {{ weather.main.temp }} °C
      </p>
      <div class="flex flex-row align-middle justify-center gap-20">
        <div class="text-center">
          <p class="text-lg">Date</p>
          <p class="text-lg">{{ date }}</p>
        </div>
        <div class="text-center">
          <p class="text-lg">Time</p>
          <p class="text-lg">{{ time }}</p>
        </div>
      </div>
      <div class="flex flex-row align-middle justify-between mt-6">
        <div class="flex flex-row align-middle justify-start gap-3">
          <img class="h-20" :src="HumidityIcon" alt="" />
          <div class="flex flex-col align-middle justify-center">
            <p class="text-lg">Humidity</p>
            <p class="text-4xl font-semibold">{{ weather.main.humidity }} %</p>
          </div>
        </div>
        <div class="flex flex-row align-middle justify-start gap-3">
          <img class="h-20" :src="WindSpeedIcon" alt="" />
          <div class="flex flex-col align-middle justify-center">
            <p class="text-lg">Wind Speed</p>
            <p class="text-4xl font-semibold">{{ weather.wind.speed }}</p>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="flex align-middle justify-center">
      <h1>ไม่พบ</h1>
    </div>
  </div>
</template>

<style scoped>
/* Add any scoped styles here */
</style>
