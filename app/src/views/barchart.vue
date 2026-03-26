 // 👇 manually update chart data
    chartData.value = {
      labels: data.value.map(item => item.name),
      datasets: [
        {
          label: "Violations by Borough",
          data: data.value.map(item => item.count),
          backgroundColor: [
            "#42A5F5",
            "#66BB6A",
            "#FFA726",
            "#EF5350",
            "#AB47BC"
          ]
        }
      ]
    };


  } catch (error) {
    console.error(error.message);
  }
}


// optional auto-load
onMounted(() => {
  getData();
});
