<script setup>
import { computed, ref } from "vue";

const props = defineProps({
  list: {
    type: Array,
    required: true,
    default: () => [],
  },
  modelValue: String,
});
const emit = defineEmits(["update:modelValue"]);
const search = ref("");
const isOpen = ref(false);

const searchResult = computed(() => {
  if (search.value === "") {
    return [];
  }
  return props.list.filter((item) => {
    return (
      item.name.toLowerCase().includes(search.value.toLowerCase()) ||
      item.enName.toLowerCase().includes(search.value.toLowerCase())
    );
  });
});

const setSelected = (item) => {
  isOpen.value = false;
  search.value = item;
  emit("update:modelValue", search.value);
  inputBox.value.setSelectionRange(
    inputBox.value.value.length,
    inputBox.value.value.length
  );
};

const handelInput = (e) => {
  search.value = e.target.value;
  isOpen.value = !!search.value; //ถ้ามีการพิพม์ข้อความให้ isOpen = true
};

const inputBox = ref(null);
const focusInput = () => {
  inputBox.value.focus();
  inputBox.value.select();
};
</script>

<template>
  <div class="px-10 flex align-middle justify-between">
    <div
      class="searchBox w-10/12 h-14 px-4 py-0 m-0 rounded-full border-2 flex items-center gap-2 relative cursor-pointer"
      @click="focusInput"
    >
      <div class="flex items-center justify-center">
        <i class="fa-solid fa-location-dot text-3xl text-primary"></i>
      </div>

      <input
        ref="inputBox"
        type="text"
        v-model="search"
        @input="handelInput"
        @click="focusInput"
        class="inputBox text-3xl text-primary font-bold px-2 py-0 w-full focus:outline-none cursor-pointer placeholder:text-xl placeholder:font-normal"
        placeholder="Enter Country"
      />

      <ul
        :class="[
          'border-2 p-2 mt-2 rounded w-11/12 max-h-80 overflow-y-auto bg-white ',
          isOpen ? 'open' : '',
        ]"
      >
        <li
          @click="setSelected(result.enName)"
          v-for="result in searchResult"
          :key="result.numeric"
          class="border-b-2 p-2 w-full cursor-pointer hover:bg-primary hover:bg-opacity-50"
        >
          {{ result.name }} ({{ result.enName }})
        </li>
      </ul>
    </div>
    <div
      class="settingBtn h-14 flex items-center justify-center rounded-full bg-primary"
    >
      <button>
        <i class="fa-solid fa-gear text-2xl text-white"></i>
      </button>
    </div>
  </div>
</template>
<style scoped>
.searchBox {
  z-index: 3;
}
.settingBtn {
  aspect-ratio: 1/1;
}
ul {
  top: 0;
  opacity: 0;
  transition: all 0.5s;
  position: absolute;
  z-index: -1;
}
ul.open {
  top: 3rem;
  opacity: 1;
  z-index: 1;
}
</style>
