<template>
  <q-page class="column justify-start items-center content-center">
    <q-file
      filled
      bottom-slots
      v-model="csvfile"
      label="Label"
      counter
      @update:model-value="changedCSV"
      :filter="filter"
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
      title="Tabelas RD"
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
const filter = ref("");
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
function changedCSV(val) {
  console.log(val);
  if (val) {
    // Create a FileReader object
    const reader = new FileReader();

    // Define what happens when the file is successfully read
    reader.onload = function (e) {
      // `e.target.result` contains the contents of the file
      const fileContents = e.target.result;
      const fileRows = fileContents.split("\n").map((row) => row.split(";"));
      const headers = fileRows[0];
      rows.value = fileRows.slice(1).map((row, index) => {
        let rowData = {};
        headers.forEach((header, i) => {
          rowData[header] = row[i];
        });
        rowData.id = index + 1; // Unique row key
        return rowData;
      });
    };

    // Read the file as text (or any other method like readAsArrayBuffer)
    reader.readAsText(val); // Here you specify which file to read
  }
}
</script>
