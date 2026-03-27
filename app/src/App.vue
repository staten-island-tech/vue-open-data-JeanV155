<template>
  <div>
    <h2>Bar Chart</h2>
    <canvas ref="barChartRef"></canvas>

    <h2>Pie Chart</h2>
    <canvas ref="pieChartRef"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import Chart from "chart.js/auto";

const data = ref([]);
const barChartRef = ref(null);
const pieChartRef = ref(null);

let barChart = null;
let pieChart = null;

async function getData() {
  const url = "https://data.cityofnewyork.us/resource/bmxf-3rd4.json";

  try {
    const response = await fetch(url);
    const result = await response.json();

    console.log(result[0]); // 🔍 check structure
    data.value = result;

    buildCharts();
  } catch (error) {
    console.error(error);
  }
}

function buildCharts() {
  if (!data.value.length) return;

  // Count shelters per borough
  const boroughCounts = {
    Manhattan: 0,
    Brooklyn: 0,
    Queens: 0,
    Bronx: 0,
    "Staten Island": 0,
  };

  data.value.forEach(item => {
    const b = item.borough; // ⚠️ check console if this doesn't work

    if (boroughCounts[b] !== undefined) {
      boroughCounts[b]++;
    }
  });

  const labels = Object.keys(boroughCounts);
  const values = Object.values(boroughCounts);

  // Destroy old charts (prevents duplicates)
  if (barChart) barChart.destroy();
  if (pieChart) pieChart.destroy();

  // BAR CHART
  barChart = new Chart(barChartRef.value, {
    type: "bar",
    data: {
      labels: labels,
      datasets: [
        {
          label: "Number of Shelters",
          data: values,
        },
      ],
    },
    options: {
      responsive: true,
    },
  });

  // PIE CHART
  pieChart = new Chart(pieChartRef.value, {
    type: "pie",
    data: {
      labels: labels,
      datasets: [
        {
          label: "Shelters Distribution",
          data: values,
        },
      ],
    },
    options: {
      responsive: true,
    },
  });
}

onMounted(getData);
</script>