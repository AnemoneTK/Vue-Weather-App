<script setup>
import { ref, watch, onMounted } from "vue";
import Icon from "./Icon.vue";
import HumidityIcon from "../assets/weatherIcon/icon_humidity.png";
import WindSpeedIcon from "../assets/weatherIcon/icon_windSpeed.png";
import SearchIcon from "../assets/img/search.png";

import Loading from "vue-loading-overlay";

// Props
const props = defineProps({
  apiKey: {
    type: String,
    required: true,
    default: "",
  },
  country: {
    type: String,
    default: "",
  },
  language: {
    type: String,
    default: "en",
  },
  units: {
    type: String,
    default: "metric",
  },
});

const weather = ref(null);
const icon = ref("");
const hour = ref(null);
const date = ref("");
const time = ref("");
const isLoading = ref(true);

// Fetch weather data and time data
const getWeather = async () => {
  isLoading.value = true;
  if (props.country) {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${props.country}&units=${props.units}&appid=${props.apiKey}&lang=${props.language}`
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
      isLoading.value = false;
    } catch (error) {
      console.error(error.message);
    }
  }
};

watch(
  () => [props.country, props.language, props.units],
  () => {
    getWeather();
  }
);

onMounted(() => {
  getWeather();
});
</script>

<template>
  <div
    class="weatherBox h-80dvh md:h-65dvh sm:px-5 md:px-5 lg:px-5 pt-0 mt-5 overflow-y-auto overflow-auto md:overflow-hidden text-primary"
  >
    <div v-if="props.country">
      <div
        v-if="isLoading"
        class="flex flex-col items-center justify-center h-full"
      >
        <loading
          v-model:active="isLoading"
          :is-full-page="true"
          color="#14121F"
          loader="bars"
        >
        </loading>
      </div>
      <div v-else>
        <div
          v-if="weather"
          class="flex flex-col items-center justify-center h-full"
        >
          <Icon :Icon="icon" :hour="hour" />
          <p class="text-lg text-center">
            {{ weather.weather[0].description }}
          </p>
          <p class="tempText my-2 md:my-4 text-center font-bold">
            {{ weather.main.temp }} {{ props.units == "metric" ? "°C" : "°F " }}
          </p>
          <div class="flex flex-row align-middle justify-center gap-20">
            <div class="text-center">
              <p class="text-lg">
                {{ props.language == "en" ? "Date" : "วันที่" }}
              </p>
              <div v-if="date">
                <p class="text-lg">{{ date }}</p>
              </div>
              <div v-else>
                <loading
                  v-model:active="isLoading"
                  :is-full-page="true"
                  color="#14121F"
                  loader="dots"
                >
                </loading>
              </div>
            </div>
            <div class="text-center">
              <p class="text-lg">
                {{ props.language == "en" ? "Time" : "เวลา" }}
              </p>
              <div v-if="time">
                <p class="text-lg">{{ time }}</p>
              </div>
              <div v-else>
                <loading
                  v-model:active="isLoading"
                  :is-full-page="true"
                  color="#14121F"
                  loader="dots"
                >
                </loading>
              </div>
            </div>
          </div>
          <div
            class="flex flex-col md:flex-row lg:flex-row xl:flex-row align-middle justify-center md:justify-center gap-5 md:gap-10 lg:justify-between lg:gap-10 xl:justify-between mt-6 lg:px-5"
          >
            <div
              class="flex flex-row align-middle justify-center md:justify-start sm:justify-center gap-3"
            >
              <img class="h-24 md:h-20 lg:h-24" :src="HumidityIcon" alt="" />
              <div class="flex flex-col align-middle justify-center">
                <p class="text-lg">
                  {{ props.language == "en" ? "Humidity" : "ความชื้น" }}
                </p>
                <p class="detailText font-semibold">
                  {{ weather.main.humidity }} %
                </p>
              </div>
            </div>
            <div
              class="flex flex-row align-middle justify-center md:justify-start sm:justify-center gap-3"
            >
              <img class="h-24 md:h-20 lg:h-24" :src="WindSpeedIcon" alt="" />
              <div class="flex flex-col align-middle justify-center">
                <p class="text-lg">
                  {{ props.language == "en" ? "Wind Speed" : "ความเร็มลม" }}
                </p>
                <p class="detailText font-semibold">{{ weather.wind.speed }}</p>
              </div>
            </div>
          </div>
        </div>
        <div v-else class="flex align-middle justify-center">
          <h1>ไม่พบ</h1>
        </div>
      </div>
    </div>
    <div v-else class="flex flex-col items-center justify-center h-full">
      <img class="h-56" :src="SearchIcon" alt="" />
      <p>{{ props.language == "en" ? "search country" : "ค้าหาประเทศ" }}</p>
    </div>
  </div>
</template>

<style scoped>
.tempText {
  font-size: clamp(48px, 2vw, 62px);
}
.detailText {
  font-size: clamp(28px, 2vw, 48px);
}
/* .weatherBox {
  height: 65dvh;
} */
</style>
