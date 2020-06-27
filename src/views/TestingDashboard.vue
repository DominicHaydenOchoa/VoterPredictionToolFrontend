<template>
  <div id="testingdashboard">
    <NavBarComponentTest></NavBarComponentTest>
    <v-main>
      <v-container>
        <v-row dense>
        <v-col cols="6">
          <testingtable></testingtable>
        </v-col>
        <v-col cols="1"></v-col>
        <v-col cols="4">
          <v-row>
            <v-col cols="12">
              <testingchartcd></testingchartcd>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <testingchartparty></testingchartparty>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <testingchartage></testingchartage>
            </v-col>
          </v-row>
        </v-col>
        </v-row>
      </v-container>
    </v-main>
  </div>
</template>>

<script>
import NavBarComponentTest from '../components/NavBarComponentTest'
import testingtable from '../components/TestingTable'
import testingchartcd from '../components/TestingChartCD'
import testingchartparty from '../components/TestingChartParty'
import testingchartage from '../components/TestingChartAge'
export default {
  name: 'TestingDashboard',
  data: function () {
    return {
    }
  },

  components: {
    NavBarComponentTest,
    testingtable,
    testingchartcd,
    testingchartparty,
    testingchartage
  }
}
</script>>
