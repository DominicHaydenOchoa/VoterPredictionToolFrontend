<template>

  <div id="trainingchartage">
    <v-card
    height="290px"
    width="700"
    :hover="true">
    <v-container>
       <vueapexchart ref="chart" type="line" width="540" height="220" :options="chartOptions" :series="series"></vueapexchart>
       <v-btn start @click="updateChart()">Load Data</v-btn>
    </v-container>
    </v-card>
  </div>
</template>

<script>
import axios from 'axios'
import vueapexchart from 'vue-apexcharts'

export default {
  name: 'trainingchartage',
  components: { vueapexchart },
  data: function () {
    return {
      series: [{
        name: 'Early Vote',
        data: []
      }],
      noData: {
        text: 'Loading...'
      },
      chartOptions: {
        chart: {
          id: 'chart',
          width: 150,
          type: 'line',
          dropShadow: {
            enabled: true,
            color: '#000',
            top: 18,
            left: 7,
            blur: 10,
            opacity: 0.2
          },
          toolbar: {
            show: false
          }
        },
        colors: ['#77B6EA', '#545454'],
        dataLabels: {
          enabled: true
        },
        stroke: {
          curve: 'smooth'
        },
        title: {
          text: 'Percentage of Early Voters by Voter Age',
          align: 'center'
        },
        grid: {
          borderColor: '#e7e7e7',
          row: {
            colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
            opacity: 0.5
          }
        },
        markers: {
          size: 1
        },
        xaxis: {
          categories: ['18-34', '35-44', '45-54', '55-64', '65-74', '75-84', 'Over 85'],
          title: {
            text: 'Voter Age'
          }
        },
        yaxis: {
          title: {
            text: 'Percentage of Early Voters'
          },
          min: 0,
          max: 100
        },
        legend: {
          position: 'top',
          horizontalAlign: 'right',
          floating: true,
          offsetY: -25,
          offsetX: -5
        },
        agetotals: null,
        agepcts: null
      }
    }
  },

  created: function () {
    axios.get('https://hevm-backend.herokuapp.com/api/training_data_count/', {
      params: { key: 'age' }
    })
      .then((response) => {
        this.agetotals = response.data

        var arr = []
        for (var i = 0; i < this.agetotals.length; i++) {
          arr.push(
            Math.floor(parseFloat(this.agetotals[i].early_vote_yes) / parseFloat(this.agetotals[i].age__count) * 100))
        }
        this.agepcts = arr
      }
      )
  },

  mounted: function () {
  },

  methods: {

    getChartData: function () {
    },

    updateChart: function () {
      console.log(this.agepcts)
      this.series = [{
        data: this.agepcts
      }]
    }
  }
}

</script>

<style>
 #trainingchart {
   background-color: #1f282c;
   background-size: cover;
 }
</style>
