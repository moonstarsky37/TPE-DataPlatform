<script setup>
import { ref, computed } from "vue";
import { useMapStore } from "../../store/mapStore";

const props = defineProps([
	"chart_config",
	"activeChart",
	"series",
	"map_config",
]);
const mapStore = useMapStore();

const chartOptions = ref({
	chart: {
		type: "bar",
		height: 350,
		stacked: true,
	},
	plotOptions: {
		bar: {
			horizontal: true,
			dataLabels: {
				total: {
					enabled: true,
					offsetX: 0,
					style: {
						fontSize: "13px",
						fontWeight: 900,
						color: '#ffffff'
					},
				},
			},
		},
	},
	stroke: {
		width: 0,
		colors: ["#282a2c"],
		show: true,
	},
	xaxis: {
		categories: props.chart_config.categories,
		axisBorder: {
			show: false,
		},
		axisTicks: {
			show: false,
		},
		labels: {
			show: false,
		},
		type: "category",
	},
	colors: props.chart_config.color,
	tooltip: {
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			return (
				'<div class="chart-tooltip">' +
				"<h6>" +
				w.globals.labels[dataPointIndex] +
				"</h6>" +
				"<span>" +
				series[seriesIndex][dataPointIndex] +
				` ${props.chart_config.unit}` +
				"</span>" +
				"</div>"
			);
		},
		followCursor: true,
	},
	fill: {
		opacity: 1,
	},
	legend: {
		position: "top",
		horizontalAlign: "left",
		offsetX: 40,
	},
	dataLabels: {
		style: {
			colors: ['#4C4C4C']
		}
	}
});

const chartHeight = computed(() => {
	return `${40 + props.series[0].data.length * 24}`;
});
</script>

<template>
	<div v-if="activeChart === 'StackBarChart'">
		<apexchart
			width="100%"
			:height="chartHeight"
			type="bar"
			:options="chartOptions"
			:series="series"
			@dataPointSelection="handleDataSelection"
		></apexchart>
	</div>
</template>
<style lang="scss" scoped></style>
