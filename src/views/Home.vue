<template>
  <v-container fluid>
    <v-row>
      <v-col sm="12" lg="7">
        <ConformityChart></ConformityChart>
      </v-col>
      <v-col sm="6" lg="2">
        <KeysStatusCard></KeysStatusCard>
      </v-col>
      <v-col sm="6" lg="3">
        <AlcaliStatusCard></AlcaliStatusCard>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <OSChartCard></OSChartCard>
      </v-col>
    </v-row>
    <v-row>
      <v-col lg="12">
        <VSChartCard></VSChartCard>
      </v-col>
    </v-row>
    <v-row>
      <v-col lg="12">
        <JobsTable :filter="{limit: 10}"></JobsTable>
      </v-col>
    </v-row>
 
  </v-container>
</template>

<script>
  // @ is an alias to /src

  import OSChartCard from "../components/OSChartCard"
  import KeysStatusCard from "../components/KeysStatusCard"
  import AlcaliStatusCard from "../components/AlcaliStatusCard"
  import ConformityChart from "../components/ConformityChart"
  import JobsTable from "../components/JobsTable"
  import VSChartCard from "../components/VSChartCard"

  export default {
    name: "home",
    components: {
      JobsTable,
      ConformityChart,
      AlcaliStatusCard,
      KeysStatusCard,
      OSChartCard,
      VSChartCard,
    },
  }
</script>
