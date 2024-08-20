<template>
  <q-page class="flex flex-center">
    <p>Current Quasar Mode: {{ quasarMode }}</p>
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    />
  </q-page>
</template>

<script setup>
import { computed } from "vue";
defineOptions({
  name: "IndexPage",
});
const quasarMode = computed(() => process.env.MODE);
</script>
