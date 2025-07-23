<script setup lang="ts">
import { defineProps } from "vue";
import { Country } from "../models/country.model";

interface Props {
  country: Country;
}

defineProps<Props>();
</script>

<template>
  <div class="bg-white dark:bg-gray-800 text-gray-800 dark:text-white shadow-md rounded-xl overflow-hidden transition-all duration-500 hover:shadow-xl hover:scale-[1.02]">
  <img :src="country.flags.png" :alt="country.flags.alt" class="h-40 w-full object-cover" />

  <div class="p-4">
    <h4 class="text-lg font-bold">{{ country.name.common }}</h4>
    <p class="text-sm"><span class="font-medium">Capital:</span> {{ country.capital?.[0] }}</p>
    <p class="text-sm"><span class="font-medium">Población:</span> {{ country.population.toLocaleString() }}</p>
    <p class="text-sm"><span class="font-medium">Región:</span> {{ country.region }}</p>
  </div>
</div>
</template>

