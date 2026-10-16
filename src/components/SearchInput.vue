<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  modelValue: { type: String, default: "" },
  placeholder: { type: String, default: "Поиск..." },
});

const emit = defineEmits(["update:modelValue"]);

const inputQuery = ref(props.modelValue);

let debounceTimeout = null;

watch(inputQuery, (newVal) => {
  clearTimeout(debounceTimeout);

  if (!newVal.trim()) {
    emit("update:modelValue", "");
    return;
  }

  debounceTimeout = setTimeout(() => {
    emit("update:modelValue", newVal);
  }, 300);
});

watch(
  () => props.modelValue,
  (newVal) => {
    inputQuery.value = newVal;
  },
);

const clearSearch = () => {
  clearTimeout(debounceTimeout);
  inputQuery.value = "";
  emit("update:modelValue", "");
};
</script>

<template>
  <div class="flex items-center border px-3 gap-2 bg-white border-gray-500/30 h-[52px] rounded-md w-[300px]">
    <svg viewBox="0 0 24 24" width="20" height="20" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M11 19C15.4183 19 19 15.4183 19 11C19 6.58172 15.4183 3 11 3C6.58172 3 3 6.58172 3 11C3 15.4183 6.58172 19 11 19Z" stroke="#263141" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      <path d="M21 21L16.65 16.65" stroke="#263141" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
    <input
      v-model="inputQuery"
      type="text"
      :placeholder="placeholder"
      class="w-full h-full outline-none text-gray-500 placeholder-gray-500 text-sm"
    />
    <button
      v-if="inputQuery"
      class="cursor-pointer"
      @click="clearSearch"
    >
      &times;
    </button>
  </div>
</template>