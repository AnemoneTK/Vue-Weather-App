<script setup>
import { computed, onMounted } from "vue";
import dRain from "../assets/weatherIcon/icon-D-rain.png";
import rain from "../assets/weatherIcon/icon_rain.png";

import brokenClouds from "../assets/weatherIcon/icon_brokenClouds.png";
import clearSky from "../assets/weatherIcon/icon_clearSky.png";
import dClearSky from "../assets/weatherIcon/icon_D-clearSky.png";

import dFewClouds from "../assets/weatherIcon/icon_D-fewClouds.png";
import fewClouds from "../assets/weatherIcon/icon_fewClouds.png";

import humidity from "../assets/weatherIcon/icon_humidity.png";
import mist from "../assets/weatherIcon/icon_mist.png";
import scatteredClouds from "../assets/weatherIcon/icon_scatteredClouds.png";
import showerRain from "../assets/weatherIcon/icon_showerRain.png";
import snow from "../assets/weatherIcon/icon_snow.png";
import thunderstorm from "../assets/weatherIcon/icon_thunderstorm.png";
import windSpeed from "../assets/weatherIcon/icon_windSpeed.png";

const props = defineProps({
  Icon: {
    type: String,
    required: true,
  },
});

const iconSrc = computed(() => {
  switch (props.Icon) {
    case "Clouds":
      return dFewClouds;
    default:
      return dRain;
  }
});
</script>
<template>
  <div>
    <img class="h-16 w-16" :src="iconSrc" alt="Weather Icon" />
  </div>
</template>
