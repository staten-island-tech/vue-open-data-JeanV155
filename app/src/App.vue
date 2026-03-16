<template>
  <div>

  </div>
</template>

<script setup>
async function getData() {
  const url = "https://data.cityofnewyork.us/resource/bmxf-3rd4.json";
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const result = await response.json();
    console.log(result);
  } catch (error) {
    console.error(error.message);
  }
}

</script>

<style  scoped>

</style>
