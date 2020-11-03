<template>
  <div class="chart">
		<canvas ref="canvas"></canvas>
	</div>
</template>

<script>
import { Line } from "vue-chartjs"
export default {
	name: 'LineChart',
    extends: Line,
    props: {

        chartData: {
            type: Array,
        },
        options: {
            type: Object,
        }
    },
    mounted() {
        let dates = this.chartData.map(d => d.date).reverse()
        let totals = this.chartData.map(d => d.total).reverse()

        this.renderChart({
            labels: dates,
            datasets: [{
                label: 'Positive',
                data: totals
            }], 
        },
        this.options
        )
    }
}
</script>

<style>

</style>