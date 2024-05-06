<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref } from "vue";

const props = defineProps(["chart_config", "activeChart", "series"]);
//console.log(props);
const chartOptions = ref({
	chart: {
		height: 390,
		type: "rangeBar",
		zoom: {
			enabled: false,
		},
	},
	colors: props.chart_config.color,
	dataLabels: {
		enabled: false,
	},
	grid: {
		xaxis: {
			lines: {
				show: true,
			},
		},
		yaxis: {
			lines: {
				show: false,
			},
		},
	},
	legend: {
		show: props.series.length > 1 ? true : false,
	},
	markers: {
		hover: {
			size: 5,
		},
		size: 5,
		strokeWidth: 1,
	},
	legend: {
		show: true,
		showForSingleSeries: true,
		position: "top",
		horizontalAlign: "left",
		customLegendItems: ["Female", "Male"],
	},
	stroke: {
		colors: props.chart_config.color,
		curve: "smooth",
		show: true,
		width: 2,
	},
	tooltip: {
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			//console.log(seriesIndex, dataPointIndex);
			//console.log(w.config.series[seriesIndex].data[dataPointIndex].y);
			// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
			return (
				'<div class="chart-tooltip">' +
				"<h6>" +
				`${parseTime(
					w.config.series[seriesIndex].data[dataPointIndex].x
				)}` +
				"</h6>" +
				"<span>" +
				w.config.series[seriesIndex].data[dataPointIndex].y[0] +
				` ${props.chart_config.unit}` +
				"</span><br>" +
				"<span>" +
				series[seriesIndex][dataPointIndex] +
				` ${props.chart_config.unit}` +
				"</span>" +
				"</div>"
			);
		},
	},
	plotOptions: {
		bar: {
			horizontal: true,
			isDumbbell: true,
			dumbbellColors: [["#EC7D31", "#36BDCB"]],
		},
	},
	fill: {
		type: "gradient",
		gradient: {
			gradientToColors: ["#36BDCB"],
			inverseColors: false,
			stops: [0, 100],
		},
	},
});

function parseTime(time) {
	return time.replace("T", " ").replace("+08:00", " ");
}
</script>

<template>
	<div v-if="activeChart === 'DumbbellChart'">
		<apexchart
			width="100%"
			height="260px"
			type="rangeBar"
			:options="chartOptions"
			:series="series"
		></apexchart>
	</div>
</template>
