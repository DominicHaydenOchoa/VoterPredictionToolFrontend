<template>

  <div id="testingchartcd">
    <v-card
    height="300px"
    width="720"
    :hover="true">
    <v-container>
       <vueapexchart ref="chart" type="line" width="560" height="230" :options="chartOptions" :series="series"></vueapexchart>
       <v-btn start @click="updateChart()">Load Data</v-btn>
    </v-container>
    </v-card>
  </div>
</template>

<script>
import axios from 'axios'
import vueapexchart from 'vue-apexcharts'

export default {
  name: 'testinghartcd',
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
          text: 'Total Number of Voters by Congressional Disctrict',
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
          categories: ['CD 1', ' CD 2', 'CD 3', 'CD 4', 'CD 5', 'CD 6', 'CD 7', 'CD 8', 'CD 9'],
          title: {
            text: 'Congressional District'
          }
        },
        yaxis: {
          title: {
            text: 'Percentage of Early Voters'
          },
          min: 0,
          max: 35
        },
        legend: {
          position: 'top',
          horizontalAlign: 'right',
          floating: true,
          offsetY: -25,
          offsetX: -5
        },
        CDtotals: null,
        CDpcts: null
      }
    }
  },

  created: function () {
    axios.get('https://hevm-backend.herokuapp.com/api/testing_data_count/', {
      params: { key: 'CD' }
    })
      .then((response) => {
        this.CDtotals = response.data

        var arr = []
        for (var i = 0; i < this.CDtotals.length; i++) {
          arr.push(this.CDtotals[i].CD__count)
        }
        this.CDpcts = arr
      }
      )
  },

  mounted: function () {
  },

  methods: {

    getChartData: function () {
    },

    updateChart: function () {
      console.log(this.CDpcts)
      this.series = [{
        data: this.CDpcts
      }]
      console.log(this.series.data)
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
