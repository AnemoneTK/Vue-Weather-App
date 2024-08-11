<script setup>
import { onMounted, onUnmounted, ref, watch } from "vue";
import ENG from "../assets/img/united-kingdom.png";
import THI from "../assets/img/thailand.png";

const isOpen = ref(false);
const settingBox = ref(null);

const handleClickOutside = (e) => {
  if (isOpen.value && !settingBox.value.contains(e.target)) {
    isOpen.value = false;
  }
};

onMounted(() => {
  document.addEventListener("click", handleClickOutside);
});
onUnmounted(() => {
  document.removeEventListener("click", handleClickOutside);
});

const props = defineProps({
  language: {
    type: String,
    default: "en",
  },
  units: {
    type: String,
    default: "metric",
  },
});
const emit = defineEmits(["update:language", "update:units"]);

const localLanguage = ref(props.language);
const localUnits = ref(props.units);

watch(localLanguage, (newValue) => {
  emit("update:language", newValue);
});

watch(localUnits, (newValue) => {
  emit("update:units", newValue);
});
</script>

<template>
  <div ref="settingBox" class="relative">
    <div
      :class="[
        'settingBtn h-14 flex items-center justify-center rounded-full ',
        isOpen ? 'open' : '',
      ]"
      @click="isOpen = !isOpen"
    >
      <button>
        <i
          :class="['icon fa-solid fa-gear text-2xl ', isOpen ? 'open' : '']"
        ></i>
      </button>
    </div>
    <div
      :class="[
        'settingBox border-2 p-2 mt-2 rounded w-50 max-h-80 overflow-y-auto bg-white flex flex-col gap-1',
        isOpen ? 'open' : '',
      ]"
    >
      <div>
        <div class="flex flex-row item-center justify-start gap-2">
          <div><i class="fa-solid fa-globe h-100"></i></div>
          <p class="text-md">Language</p>
        </div>
        <select name="language" v-model="localLanguage">
          <option value="en" :selected="props.language === 'en'">
            <img :src="ENG" alt="" />
            English
          </option>
          <option value="th" :selected="props.language === 'th'">
            <img :src="THI" alt="" />
            ภาษาไทย
          </option>
        </select>
      </div>
      <hr />
      <div>
        <div class="flex flex-row item-center justify-start gap-2">
          <div><i class="fa-solid fa-temperature-low h-100"></i></div>
          <p class="text-md">Units</p>
        </div>
        <select name="units" v-model="localUnits">
          <option value="metric" :selected="props.units === 'metric'">
            °C
          </option>
          <option value="imperial" :selected="props.units === 'imperial'">
            °F
          </option>
        </select>
      </div>
    </div>
  </div>
</template>
<style scoped>
.settingBtn {
  aspect-ratio: 1/1;
  background-color: #14121f;
  border: 2px solid #14121f;
  transition: all 0.3s ease;
}
.icon {
  color: #fff;
  transform: rotate(0deg);
  transition: all 0.3s ease;
}
.settingBtn.open {
  background-color: #fff;
}
.icon.open {
  color: #14121f;
  transform: rotate(180deg);
  transition: all 0.3s ease;
}
.settingBox {
  position: absolute;
  top: 3.1rem;
  right: 0rem;
  opacity: 0;
  transition: all 0.3s;
}
.settingBox.open {
  top: 3.5rem;
  opacity: 1;
  transition: all 0.3s;
  z-index: 3;
}
</style>
