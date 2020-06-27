<template>
  <div id="predictiontool">
      <NavBarComponentPredict></NavBarComponentPredict>
       <v-main>
           <div v-if="selected">
           <v-container>
               <v-row
               align="end"
               justify="center"
               style="height: 500px;">
               <v-col cols="1">
               </v-col>
               <v-col cols="5">
                   <v-container>
                       <v-btn
                       min-height="100"
                       min-width="300"
                       color="blue-grey lighten-4"
                       @click="handleSelectForm">
                       <span>Manually Enter Voter Info</span>
                       </v-btn>
                   </v-container>
               </v-col>
               <v-col cols="2">
                   <v-container>
                       <v-btn
                       min-height="100"
                       min-width="300"
                       color="blue-grey lighten-4"
                       @click="handleSelectSet">
                       <span>Test Whole Dataset</span>
                       </v-btn>
                   </v-container>
               </v-col>
               </v-row>
           </v-container>
           </div>
           <div id="form" v-if="showForm">
               <v-row
               align="center"
               style="height: 1000px">
                <v-col cols="1">
               </v-col>
                   <v-col cols="6">
                <v-card :dark="true">
        <v-card-title>
        Enter Voter Information :
        <v-spacer></v-spacer>
        </v-card-title>
                    <v-container>
                <form>
          <v-text-field
            v-model="name"
            :counter="30"
            label="Name"
            required
          ></v-text-field>
          <v-select
            v-model="genderSelect"
            :items="genders"
            label="Gender"
            data-vv-name="select"
            required
          ></v-select>
          <v-select
            v-model="ageSelect"
            :items="ages"
            label="Age Range"
            data-vv-name="select"
            required
          ></v-select>
          <v-select
            v-model="pctSelect"
            :items="pcts"
            label="Precinct"
            data-vv-name="select"
            required
          ></v-select>
          <v-select
            v-model="cdSelect"
            :items="cds"
            label="Congressional District"
            data-vv-name="select"
            required
          ></v-select>
          <v-select
            v-model="partySelect"
            :items="parties"
            label="Party"
            data-vv-name="select"
            required
          ></v-select>
          <div v-if="notsubmitted">
        <v-btn class="mr-4" @click="submit">submit</v-btn>
        <v-btn @click="clear">clear</v-btn>
        </div>
        <div v-if="submitted">
            <v-btn class="mr-4" @click="reset">new submission</v-btn>
        </div>
      </form>
      </v-container>
        </v-card>
      </v-col>
      <v-col cols="3">
          <v-card>
              <v-row align="center">
                  <v-col cols="10">
              <v-card-title>Prediction Results :  {{ result }}
            </v-card-title>
            </v-col>
                <v-col cols="2">
            <v-progress-circular indeterminate="true" value="20" v-if="resultLoading"></v-progress-circular>
                </v-col>
          </v-row>
          </v-card>
      </v-col>
               </v-row>
           </div>
           <div v-if="showDataset">
               <v-container>
               <v-row
               align="center"
               justify="center"
               style="height: 1000px;">
               <v-col cols="6">
                   <v-card :dark="true">
      <v-card-title>
        Model Results
        <v-btn @click="populateTable()">Run Model</v-btn>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon=""
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-card-subtitle>
        Click column title to sort
      </v-card-subtitle>
    <v-data-table
    :items="tableData"
    :headers=headers
    :items-per-page="25"
    :search="search"
    :dark="true"
    :calculate-widths="true"
    :multi-sort="true"
    :dense="true"
    no-data-text="No data to display"
    :loading="loading"
    height="500px"
    loading-text="Running Model...
    This may take a few minutes..."
    class="elevation-1">
    </v-data-table>
      </v-card>
               </v-col>
               <v-col cols="4">
                   <v-card>
                   <v-container>
                       <vueapexchart
                       ref="donut"
                       width="560"
                       height="230"
                       :options="chartOptions"
                       :series="series"></vueapexchart>
                   </v-container>
                   <v-card-subtitle>
                    Accuracy Score: {{ accuracy_score }}%
                   </v-card-subtitle>
                   </v-card>
               </v-col>
               </v-row>
           </v-container>
           </div>
       </v-main>
  </div>
</template>

<script>
import NavBarComponentPredict from '../components/NavBarComponentPredict'
import axios from 'axios'
import vueapexchart from 'vue-apexcharts'
export default {
  name: 'predictiontool',
  components: {
    NavBarComponentPredict,
    vueapexchart
  },
  data: () => {
    return {
      name: null,
      ages: ['18-34', '35-44', '45-54', '55-64', '65-74', '75-84', 'Over 85'],
      genders: ['Male', 'Female', 'Other'],
      pcts: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14',
        '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '25', '26', '27'],
      cds: ['1', ' 2', '3', '4', '5', '6', '7', '8', '9'],
      parties: ['DEM', 'GRN', 'LBT', 'OTH', 'REP'],
      selected: true,
      showDataset: false,
      showForm: false,
      result: null,
      session_id: null,
      genderSelect: null,
      ageSelect: null,
      partySelect: null,
      cdSelect: null,
      pctSelect: null,
      notsubmitted: true,
      submitted: false,
      resultLoading: false,
      tableData: [],
      search: '',
      loading: false,
      accuracy_score: null,
      headers: [
        {
          text: 'Congressional Disctrict',
          align: 'tart',
          value: 'CD'
        },
        { text: 'Precinct', value: 'precinct' },
        { text: 'Gender', value: 'gender' },
        { text: 'Age', value: 'age' },
        { text: 'Party', value: 'party' },
        { text: 'Early Voter', value: 'early_vote' },
        { text: 'Early Vote Probability Score', value: 'early_vote_score' },
        { text: 'Accuracy Result', value: 'accuracy_result' }
      ],
      series: [],
      noData: {
        text: 'Loading...'
      },
      chartOptions: {
        chart: {
          id: 'donut',
          labels: ['Accurate', 'Not Accurate'],
          width: 150,
          type: 'donut',
          dropShadow: {
            enabled: true,
            color: '#000',
            top: 18,
            left: 7,
            blur: 10,
            opacity: 0.2
          }
        },
        colors: ['#77B6EA', '#545454'],
        dataLabels: {
          enabled: true,
          formatter: function (val) {
            return val.toFixed(2) + '%'
          }
        },
        title: {
          text: 'Accuracy Report',
          align: 'center'
        },
        plotOptions: {
          pie: {
            size: 700
          }
        }
      }
    }
  },

  created: function () {
  },

  mounted: function () {
  },

  methods: {
    handleSelectForm: function () {
      this.selected = false
      this.showForm = true
      console.log('HELLO')
    },

    handleSelectSet: function () {
      this.selected = false
      this.showDataset = true
      console.log('BYE')
    },

    clear: function () {
      this.name = null
      this.genderSelect = null
      this.ageSelect = null
      this.partySelect = null
      this.cdSelect = null
      this.pctSelect = null
    },

    submit: function () {
      this.resultLoading = true
      // eslint-disable-next-line camelcase
      var session_id = parseInt(Math.random() * 1000)
      axios.post('https://hevm-backend.herokuapp.com/api/testing_data_input/', {
        session_id: session_id,
        precinct: this.pctSelect,
        CD: this.cdSelect,
        age: this.ageSelect,
        gender: this.genderSelect.substring(0, 1),
        early_vote: null,
        party: this.partySelect
      }).then((response) => {
        this.result = response.data.early_vote
        this.resultLoading = false
        this.reset()
      })
    },

    deleteResult: function () {
    },

    reset: function () {
      this.name = null
      this.genderSelect = null
      this.ageSelect = null
      this.partySelect = null
      this.cdSelect = null
      this.pctSelect = null
      this.submitted = false
      this.notsubmitted = true
    },

    populateTable: function () {
      this.loading = true
      axios.get('https://hevm-backend.herokuapp.com/api/testing_data_results/', {
        params: {
          sessionid: Math.floor(Math.random() * 100)
        }
      })
        .then((response) => {
          this.tableData = response.data
          this.loading = false
          this.populateChart()
        })
    },

    populateChart: function () {
      var count = 0
      for (var i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].accuracy_result === 'Y') {
          count++
        }
        var notcount = 150 - count
        this.series = [count, notcount]
        var percent = (count / 150) * 100
        this.accuracy_score = percent.toFixed(2)
      }
    }

  }
}

</script>

<style>

</style>
