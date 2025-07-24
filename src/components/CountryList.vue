<script setup lang="ts">
import { defineProps } from "vue";
import { Country } from "../models/country.model";
import LazyCountryItem from "./LazyCountryItem.vue"; // 👈 importante

interface Props {
  countries: Country[];
}

defineProps<Props>();
</script>

<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3">
    <div
      v-for="country in countries"
      :key="country.name.common"
      class="p-3"
    >
      <LazyCountryItem :country="country" />
    </div>
  </div>
</template>