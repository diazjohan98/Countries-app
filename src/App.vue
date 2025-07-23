<script setup lang="ts">
import { onMounted, ref } from "vue";

import PageHeader from "./components/PageHeader.vue";
import CountryList from "./components/CountryList.vue";
import axiosClient from "./utils/axios";
import { Country } from "./models/country.model";

const countries = ref<Country[]>([]);

const fetchCounries = async () => {
  try {
    const { data } = await axiosClient.get(
      "/all?fields=name,flags,population,region,capital"
    );
    countries.value = data;
  } catch (error) {
    console.log(error);
  }
};

const isDark = ref(false);

const toggleDark = () => {
  document.documentElement.classList.toggle("dark");
  isDark.value = document.documentElement.classList.contains("dark");
};

onMounted(() => {
  isDark.value = document.documentElement.classList.contains("dark");
});

onMounted(() => {
  fetchCounries();
});
</script>

<template>
 <div class="min-h-screen bg-gray-100 dark:bg-gray-900 transition-colors duration-500">
   <button
  @click="toggleDark"
  class=" rounded-full fixed top-4 right-4 bg-gray-200 dark:bg-gray-700 text-sm px-4 py-2 rounded transition-colors duration-300"
>
  {{ isDark ? 'Light Mode' : 'Dark Mode' }}
</button>
    <PageHeader />
    <div class="container mt-4 max-w-screen-lg mx-auto px-6">
      <CountryList :countries="countries" />
    </div>
  </div>
</template>
