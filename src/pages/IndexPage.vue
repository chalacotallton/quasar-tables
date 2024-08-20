<template>
  <q-page class="flex flex-center">
    <q-table
      flat
      bordered
      title="Treats"
      :rows="rows"
      :columns="columns"
      no-data-label="I didn't find anything for you"
      row-key="name"
    />
  </q-page>
</template>

<script setup>
import { ref } from "vue";
defineOptions({
  name: "IndexPage",
});
const rows = ref([]);
columns: [
  {
    name: "name",
    required: true,
    label: "name",
    align: "left",
    field: "name",
    sortable: true,
  },
  {
    name: "type",
    align: "center",
    label: "type",
    field: "type",
    sortable: true,
  },
  { name: "database", label: "database", field: "database", sortable: true },
  { name: "table", label: "table", field: "table", sortable: true },
];
</script>
