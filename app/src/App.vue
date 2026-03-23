<template>
  <div>
    <div v-for="(item, index) in data" :key="index">
      {{ item }}
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";


const data = ref([]);

async function getData() {
  const url = "https://data.cityofnewyork.us/resource/bmxf-3rd4.json";

  try {
    const response = await fetch(url);
    const result = await response.json();

    data.value = result;

    console.log(data.value); 
  } catch (error) {
    console.error(error);
  }
}

onMounted(() => {
  getData();
});
</script>

<style scoped>


</style>