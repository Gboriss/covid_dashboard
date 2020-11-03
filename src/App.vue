<template>
<div id="app" class="container">
	<div class="row mt-5" v-if="arrPositive.length > 0">
		<div class="col">
			<h2>Positive</h2>
			<LineChart 
				:chartData="arrPositive"
				:options="chartOptions"	
			/>
		</div>
	</div>
</div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import LineChart from '@/components/LineChart.vue'

export default {
	name: 'app',
	components: {
		LineChart
	},
	data() {
		return {
			arrPositive: [],
			arrHospitalized: [],
			arrInIcu: [],
			arrOnventilators: [],
			arrRecovered: [],
			arrDeaths: [],
			chartOptions: {
				responsive: true,
				maintainAspectRatio: false
			}
		}
	},
	async created() {
		const { data } = await axios.get('https://covidtracking.com/api/us/daily')
		console.log(data)
		data.forEach(d => {
			const date = moment(d.date, 'YYYYMMDD').format('DD/MM')

			const {
				positive,
				hospitalizedCurrently,
				inIcuCurrently,
				onVentilatorCurrently,
				recovered,
				death
			} = d

			this.arrPositive.push({date, total: positive})
			this.arrHospitalized.push({date, total: hospitalizedCurrently})
			this.arrInIcu.push({date, total: inIcuCurrently})
			this.arrOnventilators.push({date, total: onVentilatorCurrently})
			this.arrRecovered.push({date, total: recovered})
			this.arrDeaths.push({date, total: death})

			// console.log(this.arrPositive)
		})
	}
}
</script>

<style lang="scss">

</style>
