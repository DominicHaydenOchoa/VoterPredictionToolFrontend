<template>
  <div>
  <v-app id="TrainingTable">
    <v-card :dark="true">
      <v-card-title>
        Model Training Data
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
    height="800px"
    loading-text="Loading Model Testing Data..."
    class="elevation-1">
    </v-data-table>
      </v-card>
  </v-app>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'trainingtable',
  components: {
  },

  data: function () {
    return {
      tableData: [],
      search: '',
      loading: false,
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
        { text: 'Early Voter', value: 'early_vote' }
      ]
    }
  },

  created: function () {
    this.populateTable()
  },

  methods: {
    populateTable: function () {
      this.loading = true
      axios.get('https://hevm-backend.herokuapp.com/api/training_data/')
        .then((response) => {
          this.tableData = response.data
          this.loading = false
        })
    }
  }
}

</script>

<style>
 #TrainingTable {
   background-color: #1f282c;
   background-size: cover;
 }
</style>
