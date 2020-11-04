<template>
<div id="app" class="container">
	<h1>Covid-19 Data Visualization</h1>
	<div class="graf" v-if="arrPositive.length > 0">
		<LineChart 
			:chartData="arrPositive"
			label="Positive"
			:chartColors="positiveChartColors"
		/>
	</div>
	<div class="graf" v-if="arrHospitalized.length > 0">
		<LineChart 
			:chartData="arrHospitalized"
			label="Hospitalized"
			:chartColors="hospitalizedChartColors"
		/>
	</div>
	<div class="graf" v-if="arrInIcu.length > 0">
		<LineChart 
			:chartData="arrInIcu"
			label="In Icu"
			:chartColors="inIcuColors"
		/>
	</div>
	<div class="graf" v-if="arrOnventilators.length > 0">
		<LineChart 
			:chartData="arrOnventilators"
			label="On ventilators"
			:chartColors="onventilatorsColors"
		/>
	</div>
	<div class="graf" v-if="arrRecovered.length > 0">
		<LineChart 
			:chartData="arrRecovered"
			label="Recovered"
			:chartColors="recoveredColors"
		/>
	</div>
	<div class="graf" v-if="arrDeaths.length > 0">
		<LineChart 
			:chartData="arrDeaths"
			label="Deaths"
			:chartColors="deathsColors"
		/>
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
			positiveChartColors: {
				borderColor: '#077178', 
                pointBorderColor: '#0E1428', 
                pointBackgroundColor: '#AFD6AC', 
                backgroundColor: '#74A57F'
			},
			arrHospitalized: [],
			hospitalizedChartColors: {
				borderColor: '#251F47', 
                pointBorderColor: '#260F26', 
                pointBackgroundColor: '#858EAB', 
                backgroundColor: '#858EAB'
			},
			arrInIcu: [],
			inIcuColors: {
				borderColor: '#190B28', 
                pointBorderColor: '#190B28', 
                pointBackgroundColor: '#E55381', 
                backgroundColor: '#E55381'
			},
			arrOnventilators: [],
			onventilatorsColors: {
				borderColor: '#784F41', 
                pointBorderColor: '#784F41', 
                pointBackgroundColor: '#BBE5ED', 
                backgroundColor: '#BBE5ED'
			},
			arrRecovered: [],
			recoveredColors: {
				borderColor: '#4E5E66', 
                pointBorderColor: '#4E5E66', 
                pointBackgroundColor: '#31E981', 
                backgroundColor: '#31E981'
			},
			arrDeaths: [],
			deathsColors: {
				borderColor: '#E06D06', 
                pointBorderColor: '#E06D06', 
                pointBackgroundColor: '#402A2C', 
                backgroundColor: '#402A2C'
			},
		}
	},
	async created() {
		const { data } = await axios.get('https://covidtracking.com/api/us/daily')
		// console.log(data)
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
		})
	}
}
</script>

<style lang="scss">
body {
	margin: 0;
	background-color: #ffeb3b75;
	
}

.container {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	
	text-align: center;
	margin-top: 60px;
	max-width: 1000px;
	margin: 0 auto;
}
.graf {
	margin: 40px 0;
}

</style>
