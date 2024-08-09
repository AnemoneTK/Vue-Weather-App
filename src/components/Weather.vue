<script setup>
import { ref } from "vue";
import humidity from "../assets/weatherIcon/icon_humidity.png";
import windSpeed from "../assets/weatherIcon/icon_windSpeed.png";
import Icon from "./Icon.vue";
//props
const props = defineProps({
  apiKey: {
    type: String,
    required: false,
  },
});

const city = ref("");
const weather = ref(null);
const hour = ref(null);
const date = ref("");
const time = ref("");

const getWeather = async () => {
  const response = await fetch(
    `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=${props.apiKey}`
  );
  const data = await response.json();
  weather.value = data;

  //เก็บค่า ละติจูด ลองจิจูด เพื่อนำไปหาตำแหน่งที่ตั้ง แะล Timezone
  const lat = data.coord.lat;
  const lon = data.coord.lon;

  //นำ ละติจูด ลองจิจูด ไปใช้ในการหาเวลา
  const timeResponse = await fetch(
    `http://localhost:3000/api/time?lat=${lat}&lon=${lon}`
  );
  const timeData = await timeResponse.json();
  hour.value = timeData.hour;
  date.value = timeData.date;
  time.value = timeData.time;
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
      class="bg-white border border-primary text-primary p-2 rounded mt-2 w-full"
    >
      Get Weather
    </button>
    <div v-if="weather">
      <h2 class="text-2xl font-bold mt-4">{{ weather.name }}</h2>
      <p class="text-lg">{{ weather.weather[0].description }}</p>
      <p class="text-lg">Temp: {{ weather.main.temp }} °C</p>
      <p class="text-lg">Humidity: {{ weather.main.humidity }}%</p>
      <p class="text-lg">Date: {{ date }}</p>
      <p class="text-lg">Time: {{ time }}</p>
      <p class="text-lg">Hour: {{ hour }}</p>
      <p class="text-lg">{{ weather.weather[0].main }}</p>
      <Icon :Icon="weather.weather[0].icon" :hour="hour" />
    </div>
  </div>
</template>

<style scoped>
/* Add any scoped styles here */
</style>
