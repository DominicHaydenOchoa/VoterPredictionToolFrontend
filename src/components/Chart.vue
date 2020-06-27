<script>
import { Line } from 'vue-chartjs'
export default {
  extends: Line,
  props: {
    chartdata: {
      type: Array,
      default: null
    },
    options: {
      type: Array,
      default: null
    }
  },
  mounted () {
    this.renderChart(this.chartdata, this.options)
  }
}
</script>>
