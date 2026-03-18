<template>
  <div>
    <canvas id="myChart"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import Chart from "chart.js/auto";

const data = ref([]);

async function getData() {
  const url = "https://data.cityofnewyork.us/resource/bmxf-3rd4.json";

  try {
    const response = await fetch(url);
    const result = await response.json();

    data.value = result;

    console.log(data.value[0]); // 👈 check structure if needed

    makeChart(); // 👈 build chart AFTER data loads
  } catch (error) {
    console.error(error);
  }
}

function makeChart() {
  let brooklyn = 0;
  let manhattan = 0;
  let queens = 0;
  let bronx = 0;
  let statenIsland = 0;

  data.value.forEach(item => {
    const boro = item.borough?.toLowerCase();

    if (boro === "brooklyn") brooklyn++;
    else if (boro === "manhattan") manhattan++;
    else if (boro === "queens") queens++;
    else if (boro === "bronx") bronx++;
    else if (boro === "staten island") statenIsland++;
  });

  const ctx = document.getElementById("myChart");

  new Chart(ctx, {
    type: "bar",
    data: {
      labels: ["Brooklyn", "Manhattan", "Queens", "Bronx", "Staten Island"],
      datasets: [
        {
          label: "Number of Shelters",
          data: [brooklyn, manhattan, queens, bronx, statenIsland]
        }
      ]
    }
  });
}

// 👇 THIS is what starts everything
onMounted(() => {
  getData();
});
</script>

<style scoped>
canvas {
  max-width: 600px;
  margin: auto;
}
</style>