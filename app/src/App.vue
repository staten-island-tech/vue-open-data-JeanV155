<template>
  <div>
    <BarChart :shelters="data" />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import barchart from "./components/barchart.vue";

const data = ref([]);

async function getData() {
  const url = "https://data.cityofnewyork.us/resource/bmxf-3rd4.json";

  try {
    const response = await fetch(url);
    const result = await response.json();

    data.value = result;
  } catch (error) {
    console.error(error);
  }
}

onMounted(() => {
  getData();
});
</script>