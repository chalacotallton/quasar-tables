<template>
  <q-page class="row justify-start items-start content-start">
    <div class="q-gutter-sm col-3 column q-pa-sm">
      <q-list dense bordered padding>
        <div class="row">
          <q-item class="col-6">
            <q-item-section side top>
              <q-checkbox
                size="sm"
                v-model="desmarcarTudo"
                color="teal"
                :style="{ fontSize: '0.75rem' }"
              />
            </q-item-section>
            <q-item-section>
              <q-item-label>Desmarcar Todos</q-item-label>
            </q-item-section>
          </q-item>
          <q-item class="col-6">
            <q-item-section side top>
              <q-checkbox
                size="sm"
                v-model="marcarTudo"
                color="teal"
                :style="{ fontSize: '0.75rem' }"
              />
            </q-item-section>
            <q-item-section>
              <q-item-label>Marcar Todos</q-item-label>
            </q-item-section>
          </q-item>
        </div>
        <q-item
          tag="label"
          v-ripple
          v-for="db in dbs"
          :key="db"
          class="q-gutter-none q-pb-none q-mb-none"
        >
          <q-item-section side top>
            <q-checkbox
              size="sm"
              v-model="selection"
              :val="db"
              color="teal"
              :style="{ fontSize: '0.75rem' }"
            />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ db }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </div>
    <div class="column col">
      <q-file
        filled
        bottom-slots
        v-model="csvfile"
        label="Upload CSV"
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
        title="Tabelas RD"
        :rows="rows"
        :columns="columns"
        no-data-label="I didn't find anything for you"
        row-key="id"
        :filter="filter"
        style="width: 100%"
        :loading="loading"
      >
        <template v-slot:top>
          <q-space />
          <q-input
            borderless
            dense
            debounce="300"
            color="primary"
            v-model="filter"
            placeholder="Filtrar"
          >
            <template v-slot:append>
              <q-icon name="search" />
            </template>
          </q-input>
        </template>
        <template v-slot:loading>
          <q-inner-loading showing color="primary" />
        </template>
      </q-table>
    </div>
  </q-page>
</template>

<script setup>
import { ref, watch, computed } from "vue";
defineOptions({
  name: "IndexPage",
});
const rows = computed(() => {
  return originalRows.value.filter((row) =>
    selection.value.includes(row.database)
  );
});
const filter = ref("");
const originalRows = ref([]);
const selection = ref([]);
const dbs = ref([]);
const desmarcarTudo = ref(false);
const marcarTudo = ref(true);
const loading = ref(false);
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
  if (val) {
    loading.value = true;
    // Create a FileReader object
    const reader = new FileReader();

    // Define what happens when the file is successfully read
    reader.onload = function (e) {
      // `e.target.result` contains the contents of the file
      const fileContents = e.target.result;
      const fileRows = fileContents.split("\n").map((row) => row.split(";"));
      const headers = fileRows[0];
      originalRows.value = fileRows.slice(1).map((row, index) => {
        let rowData = {};
        headers.forEach((header, i) => {
          rowData[header] = row[i];
        });
        rowData.id = index + 1; // Unique row key
        return rowData;
      });
      const uniqueDbsArray = Array.from(
        new Set(originalRows.value.map((el) => el.database).filter((el) => el))
      );
      dbs.value = uniqueDbsArray;
      selection.value = uniqueDbsArray;
      loading.value = false;
    };

    // Read the file as text (or any other method like readAsArrayBuffer)
    reader.readAsText(val); // Here you specify which file to read
  }
}

watch(desmarcarTudo, (newValue) => {
  if (newValue) {
    selection.value = []; // Reset selection when desmarcarTudo is true
    marcarTudo.value = false;
  }
});
watch(marcarTudo, (newValue) => {
  if (newValue) {
    const uniqueDbsArray = Array.from(
      new Set(originalRows.value.map((el) => el.database).filter((el) => el))
    );
    selection.value = uniqueDbsArray;
    desmarcarTudo.value = false;
  }
});
</script>
