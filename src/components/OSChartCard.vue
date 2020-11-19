<template>
  <v-container fluid>
   
      <canvas ref="chart"></canvas>
    
  </v-container>
</template>

<script>
  import Chart from "chart.js"
  import { Doughnut } from 'vue-chartjs'
  
  export default {
    extend: Doughnut,
    name: "OSChartCard",
    props: ["minion"],
    data() {
      return {
      
        jobchart: null,
        labels: null,
        chart_data: [],
      }
    },
    mounted() {
      this.createChart()
    },
    methods: {
      loadData() {
        let params = { params: {  } }
        if (this.minion) {
          params.params.id = this.minion
        }
        this.$http.get("api/os/graph", params).then(response => {
          this.jobchart.data.labels = response.data.labels
          this.jobchart.data.data = response.data.series
          this.jobchart.update()
        })
      },
      createChart() {

        let params = { params: { } }
        if (this.minion) {
          params.params.id = this.minion
        }
        if (this.jobchart != null) {
          this.jobchart.destroy()
        }
        this.$http.get("api/os/graph", params).then(response => {
          this.labels = response.data.labels
          this.chart_data = response.data.series
          this.$refs.chart.height = 60
          this.jobchart = new Chart(this.$refs.chart, {
            type: "doughnut",
            data: {
              message: this.labels,
              labels: this.labels,
              datasets: [{
                lineTension: 0.1,
                pointRadius: 1,
                data: this.chart_data, // fake data before update(needed for plugin).
                fill: false,
                colorStart: "rgba(0, 173, 238, 1.0)",
                colorEnd: "rgba(231, 18, 143, 1.0)",
              },]
            },
            options: {
              legend: {
                display: true
              },
              responsive: true,
            }
          })
        })
      },
    },
  }
</script>

<style scoped>

</style>