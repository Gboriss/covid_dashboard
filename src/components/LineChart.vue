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
        label: {
            type: String
        },
        chartColors: {
            type: Object
        }
    },
    mounted() {
        let dates = this.chartData.map(d => d.date).reverse()
        for (let i = 0; i < dates.length; i++) {
            dates.splice(i + 1, 8)
        }
        let totals = this.chartData.map(d => d.total).reverse()
        for (let i = 0; i < totals.length; i++) {
            totals.splice(i + 1, 8)
        }

        const {borderColor, pointBorderColor, pointBackgroundColor, backgroundColor} = this.chartColors
        
        this.renderChart({
            labels: dates,
            datasets: [{
                label: this.label,
                // backgroundColor: '#f34',
				// pointBackgroundColor: "#4a148c",
                data: totals,
                borderColor: borderColor, 
                pointBorderColor: pointBorderColor, 
                pointBackgroundColor: pointBackgroundColor, 
                backgroundColor: backgroundColor
            }], 
        },
        {   
            responsive: true, 
            maintainAspectRatio: true, 

			scales: {
					xAxes: [
					{
						ticks: {
                            autoSkip: true,
							maxTicksLimit: 20,
                            
                        },
        
					}
				    ]
                }   
            },
        )
    }
}
</script>

<style>
.chart {
    /* max-width: 1000px; */
    /* margin: 0 100px 50px;
    padding: 0 50px; */
}

</style>