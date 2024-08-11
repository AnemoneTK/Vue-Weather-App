<script setup>
import { computed, onMounted } from "vue";

import dRain from "../assets/weatherIcon/icon-D-rain.png";
import rain from "../assets/weatherIcon/icon_rain.png";

import clearSky from "../assets/weatherIcon/icon_clearSky.png";
import dClearSky from "../assets/weatherIcon/icon_D-clearSky.png";

import dFewClouds from "../assets/weatherIcon/icon_D-fewClouds.png";
import fewClouds from "../assets/weatherIcon/icon_fewClouds.png";

import brokenClouds from "../assets/weatherIcon/icon_brokenClouds.png";

import mist from "../assets/weatherIcon/icon_mist.png";
import scatteredClouds from "../assets/weatherIcon/icon_scatteredClouds.png";
import showerRain from "../assets/weatherIcon/icon_showerRain.png";
import snow from "../assets/weatherIcon/icon_snow.png";
import thunderstorm from "../assets/weatherIcon/icon_thunderstorm.png";

const props = defineProps({
  Icon: {
    type: String,
    required: true,
  },
  hour: {
    type: Number,
    required: false,
  },
});

const iconSrc = computed(() => {
  switch (props.Icon) {
    case "01d":
      return dClearSky;
    case "01n":
      return clearSky;
    case "02d":
      return dFewClouds;
    case "02n":
      return fewClouds;
    case "03d":
      return scatteredClouds;
    case "03n":
      return scatteredClouds;
    case "04d":
      return brokenClouds;
    case "04n":
      return brokenClouds;
    case "09d":
      return showerRain;
    case "09n":
      return showerRain;
    case "10d":
      return dRain;
    case "10n":
      return rain;
    case "11d":
      return thunderstorm;
    case "11n":
      return thunderstorm;
    case "13d":
      return snow;
    case "13n":
      return snow;
    case "50d":
      return mist;
    case "50n":
      return mist;
    default:
      return dRain;
  }
});
</script>
<template>
  <div class="flex justify-center">
    <img :src="iconSrc" alt="Weather Icon" />
  </div>
</template>
<style scoped>
img {
  height: auto;
  max-height: 30dvh;
}
</style>
