<template>
  <div id="dashboard">
    <NavBarComponent></NavBarComponent>
    <v-main>
      <v-container>
        <v-row dense>
        <v-col cols="6">
          <trainingtable></trainingtable>
        </v-col>
        <v-col cols="1"></v-col>
        <v-col cols="4">
          <v-row>
            <v-col cols="12">
              <trainingchart></trainingchart>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <trainingchartparty></trainingchartparty>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <trainingchartage></trainingchartage>
            </v-col>
          </v-row>
        </v-col>
        </v-row>
      </v-container>
    </v-main>
  </div>
</template>>

<script>
import NavBarComponent from '../components/NavBarComponent'
import trainingtable from '../components/TrainingTable'
import trainingchart from '../components/TrainingChart'
import trainingchartparty from '../components/TrainingChartParty'
import trainingchartage from '../components/TrainingChartAge'
export default {
  name: 'Dashboard',
  data: function () {
    return {
    }
  },

  components: {
    NavBarComponent,
    trainingtable,
    trainingchart,
    trainingchartparty,
    trainingchartage
  }
}
</script>>
