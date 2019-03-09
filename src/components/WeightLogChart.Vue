<script>
import { Line } from 'vue-chartjs'

export default {
  name: 'WeightLogChart',
  extends: Line,
  props: ['chartData', 'chartLabels', 'options', 'weightLogChartKey'],
  mounted () {
    this.renderLineChart()
  },
  computed: {
    cData: function () {
      return this.chartData
    }
  },
  methods: {
    renderLineChart: function () {
      this.renderChart({
        labels: this.chartLabels,
        datasets: [
          {
            label: 'Weight in lbs',
            borderColor: 'grey',
            pointBackgroundColor: 'grey',
            pointBorderColor: 'grey',
            borderWidth: 1,
            data: this.cData
          }
        ]
      },
    {responsive: true, maintainAspectRatio: false})
    }
  }
}
</script>
<style>
</style>
