<!-- src/components/LazyCountryItem.vue -->
<script setup lang="ts">
import { ref, onMounted } from "vue";
import { Country } from "../models/country.model";
import CountryItem from "./CountryItem.vue";

const props = defineProps<{
  country: Country;
}>();

const isVisible = ref(false);
const target = ref<HTMLElement | null>(null);

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry], obs) => {
      if (entry.isIntersecting) {
        isVisible.value = true;
        obs.disconnect();
      }
    },
    {
      rootMargin: "100px",
    }
  );

  if (target.value) observer.observe(target.value);
});
</script>

<template>
  <div ref="target" class="min-h-[280px]">
    <CountryItem v-if="isVisible" :country="country" />
  </div>
</template>
