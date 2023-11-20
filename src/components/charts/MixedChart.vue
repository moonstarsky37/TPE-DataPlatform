<script setup>
import { ref, computed } from "vue";
import { useMapStore } from "../../store/mapStore";

const props = defineProps([
	"chart_config",
	"activeChart",
	"series",
	"map_config",
]);
const parseSeries = computed(() =>
	props.series.map((s) => ({
		...s,
		type: props.chart_config.chart_type[s.name],
	}))
);

const chartOptions = ref({
	chart: {
		height: 350,
		type: "line",
		stacked: false,
	},
	colors: props.chart_config.color,
	dataLabels: {
		enabled: false,
	},
	stroke: {
		width: [4, 4, 4],
	},
	title: {
		text: "",
		align: "left",
		offsetX: 110,
	},
	xaxis: {
		categories: props.chart_config.categories,
	},
	yaxis: props.series.map((s, i) => ({
		opposite: i !== 0,
		axisTicks: {
			show: true,
		},
		axisBorder: {
			show: true,
			color: props.chart_config.color[i],
		},
		labels: {
			style: {
				colors: props.chart_config.color[i],
			},
			formatter: (v) => Math.round(v),
		},
		title: {
			text: s.name,
			style: {
				color: props.chart_config.color[i],
			},
		},
		width: 50,
	})),
	tooltip: {
		// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			return (
				'<div class="chart-tooltip">' +
				"<h6>" +
				w.globals.labels[dataPointIndex] +
				`${
					props.chart_config.categories
						? "-" + w.globals.seriesNames[seriesIndex]
						: ""
				}` +
				"</h6>" +
				"<span>" +
				series[seriesIndex][dataPointIndex].toFixed(2) +
				` ${props.chart_config.unit}` +
				"</span>" +
				"</div>"
			);
		},
		fixed: {
			enabled: false,
		},
	},
});
</script>

<template>
	<div v-if="activeChart === 'MixedChart'">
		<apexchart
			type="line"
			height="270"
			:options="chartOptions"
			:series="parseSeries"
		></apexchart>
	</div>
</template>

<style lang="scss" scoped></style>
