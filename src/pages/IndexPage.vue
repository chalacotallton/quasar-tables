<template>
  <q-page class="flex flex-center">
    <q-file
      filled
      bottom-slots
      v-model="csvfile"
      label="Label"
      counter
      @update:model-value="changedCSV"
    >
      <template v-slot:prepend>
        <q-icon name="cloud_upload" @click.stop.prevent />
      </template>
      <template v-slot:append>
        <q-icon
          name="close"
          @click.stop.prevent="model = null"
          class="cursor-pointer"
        />
      </template>

      <template v-slot:hint> Field hint </template>
    </q-file>
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
const columns = ref([
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
]);
const csvfile = ref(null);
function changedCSV() {
  alert("hh");
}
</script>
