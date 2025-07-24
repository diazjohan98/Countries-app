<script setup lang="ts">
import { ref } from "vue";


const search = ref("")

const props = defineProps<{
  modelValue: string;
  placeholder?: string;
}>();
const emit = defineEmits(["update:modelValue"]);

const model = defineModel<string>("modelValue");


</script>


<template>
  <label class="relative block w-full">
    <span class="sr-only">Search</span>
    <span class="absolute inset-y-0 left-0 flex items-center pl-2">
      <svg
        class="h-5 w-5 fill-slate-400 dark:fill-slate-300"
        viewBox="0 0 20 20"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          fill-rule="evenodd"
          d="M12.9 14.32a8 8 0 111.414-1.414l4.387 4.387a1 1 0 01-1.414 1.414l-4.387-4.387zM8 14a6 6 0 100-12 6 6 0 000 12z"
          clip-rule="evenodd"
        />
      </svg>
    </span>
    <input
      v-model="model"
      class="placeholder:italic placeholder:text-slate-400 dark:placeholder:text-slate-500 block w-full rounded-md bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-600 py-2 pl-9 pr-3 shadow-sm focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 sm:text-sm text-slate-900 dark:text-slate-100"
      :placeholder="placeholder"
      type="text"
      name="search"
     
    />
  </label>
</template>