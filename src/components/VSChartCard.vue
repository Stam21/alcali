<template >
<v-container fluid>
  <v-card :elevation="minion == null ? 2 : 0">
      <v-list-item three-line dense>
        <v-list-item-content>
          <v-list-item-title class="headline mb-1">Jobs Stats</v-list-item-title>
        </v-list-item-content>
        <v-spacer></v-spacer>
        <v-list-item-content class="mr-6">
          <v-select
              :items="filters"
              label="Filter"
              v-model="selectedFilter"
              @change="loadData"
              return-object
          ></v-select>
        </v-list-item-content>
        <v-list-item-content>
          <v-select
              :items="periods"
              label="Period"
              v-model="selectedPeriod"
              @change="loadData"
              return-object
          ></v-select>
        </v-list-item-content>

      </v-list-item>
    <v-row class="content">
        <div>
        <canvas id="chart1" ref="chart"></canvas>
        </div>
        <div>
        <canvas id="chart2" ref="chart2"></canvas>
        </div>
        <div>
        <canvas id="chart3" ref="chart3"></canvas>
        </div>
    </v-row>
  </v-card>
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
        this.$http.get("api/vs/graph", params).then(response => {
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
        this.$http.get("api/vs/graph", params).then(response => {
          this.labels = response.data.labels
          this.chart_data = response.data.series
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
        this.$http.get("api/vs/graph", params).then(response => {
          this.labels = response.data.labels
          this.chart_data = response.data.series  
          this.jobchart = new Chart(this.$refs.chart3, {
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
        this.$http.get("api/vs/graph", params).then(response => {
          this.labels = response.data.labels
          this.chart_data = response.data.series
          this.jobchart = new Chart(this.$refs.chart2, {
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
    .content{
    justify-content: space-between
    }
</style>