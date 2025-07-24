<script setup lang="ts">
import { onMounted, ref, watch } from "vue";

import PageHeader from "./components/PageHeader.vue";
import CountryList from "./components/CountryList.vue";
import SearchInput from "./components/shared/SearchInput.vue";
import axiosClient from "./utils/axios";
import { Country } from "./models/country.model";
import noCountriesImg from "./assets/undraw_not-found_6bgl.svg";

const countries = ref<Country[]>([]);
const search = ref("");
const filteredCountries = ref<Country[]>([]);
const page = ref(1);
const itemPerPage = ref(12);
const paginatedCountries = ref<Country[]>([]);
const totalItems = ref(0);

const fetchCounries = async () => {
  try {
    const { data } = await axiosClient.get(
      "/all?fields=name,flags,population,region,capital"
    );
    countries.value = data;
    totalItems.value = countries.value.length;
  } catch (error) {
    console.log(error);
  }
};

const isDark = ref(false);

const toggleDark = () => {
  document.documentElement.classList.toggle("dark");
  isDark.value = document.documentElement.classList.contains("dark");
};

const sliceCountries = (currentCountries: Country[]) => {
  const start = (page.value - 1) * itemPerPage.value;
  const end = page.value * itemPerPage.value;
  paginatedCountries.value = currentCountries.slice(start, end);
};
onMounted(() => {
  isDark.value = document.documentElement.classList.contains("dark");
});

const filterCountries = () => {
  filteredCountries.value = countries.value.filter((country) =>
    country.name.common.toLowerCase().includes(search.value.toLowerCase())
  );
};

const changePage = (newPage: number) => {
  page.value = newPage;
};

onMounted(() => {
  fetchCounries();
});

watch([countries, filteredCountries, search, page], () => {
  const currentList =
    filteredCountries.value.length <= 0 && search.value === ""
      ? countries.value
      : filteredCountries.value;

  sliceCountries(currentList);
  totalItems.value = currentList.length;
});
</script>

<template>
  <div
    class="min-h-screen bg-gray-100 dark:bg-gray-900 transition-colors duration-500"
  >
    <button
      @click="toggleDark"
      class="rounded-full fixed top-4 right-4 bg-gray-200 dark:bg-gray-700 text-sm px-4 py-2 rounded transition-colors duration-300"
    >
      {{ isDark ? "Light Mode" : "Dark Mode" }}
    </button>
    <PageHeader />
    <div class="container mt-4 max-w-screen-lg mx-auto px-6">
      <SearchInput
        v-model="search"
        placeholder="Search by countries name"
        @input="filterCountries"
      />
      <div class="mt-8 mb-8 flex justify-center space-x-6">
        <button
          :disabled="page <= 1"
          :class="{ 'opacity-50': page <= 1 }"
          @click="changePage(page - 1)"
          class="border border-gray-300 dark:border-gray-700 rounded px-2 py-0.5 dark:fill-slate-300 bg-white dark:bg-gray-800 text-gray-800 dark:text-white hover:bg-gray-200 dark:hover:bg-sky-800"
        >
          Previous
        </button>
        <button
          :disabled="page >= Math.ceil(totalItems / itemPerPage)"
          :class="{ 'opacity-50': page >= Math.ceil(totalItems / itemPerPage) }"
          @click="changePage(page + 1)"
          class="border border-gray-300 dark:border-gray-700 rounded px-2 py-0.5 dark:fill-slate-300 bg-white dark:bg-gray-800 text-gray-800 dark:text-white hover:bg-gray-200 dark:hover:bg-sky-800"
        >
          Next
        </button>
      </div>
      <CountryList :countries="paginatedCountries" />

      <div
        v-if="paginatedCountries.length === 0"
        class="flex flex-col items-center justify-center mt-12 text-center"
      >
        <img
          :src="noCountriesImg"
          alt="No hay países"
          class="w-64 h-64 dark:invert"
        />
        <p class="mt-4 text-lg text-gray-500 dark:text-gray-300">
          No countries
        </p>
      </div>
    </div>
  </div>
</template>
