<template>
  <div id="dashboard">
    <v-app>
      <v-main>
        <v-layout row wrap>
          <v-flex xs4 md2>
            <NavBarComponent></NavBarComponent>
          </v-flex>
          <v-flex xs8 md10>
              <v-layout row wrap>
                <v-flex xs4 md6>
              <trainingtable></trainingtable>
                </v-flex>
                <v-flex xs4 md4>
                  <trainingchart></trainingchart>
                </v-flex>
              </v-layout>
          </v-flex>
        </v-layout>
        </v-main>
    </v-app>
  </div>
</template>>

<script>
import NavBarComponent from '../components/NavBarComponent'
import trainingtable from '../components/TrainingTable'
import trainingchart from '../components/TrainingChart'
export default {
  name: 'Dashboard',
  data: function () {
    return {
    }
  },

  components: {
    NavBarComponent,
    trainingtable,
    trainingchart
  }
}
</script>>
