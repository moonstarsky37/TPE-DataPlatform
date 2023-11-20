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
const data = computed(() => props.series[0].data.map(d => d.y))

const chartOptions = ref({
	stroke: {
		colors: props.chart_config.color,
	},
	legend: {
		position: "bottom"
	},
	fill: {
		opacity: 0.8,
	},
	labels: props.series[0].data.map(d => d.x),
	tooltip: {
		followCursor: false,
		custom: function ({ series, seriesIndex, w }) {
			// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
			return '<div class="chart-tooltip">' +
				'<h6>' + w.globals.labels[seriesIndex] + '</h6>' +
				'<span>' + series[seriesIndex] + ` ${props.chart_config.unit}` + '</span>' +
				'</div>';
		},
	},
	responsive: [
		{
			breakpoint: 480,
			options: {
				chart: {
					width: 200,
				},
				legend: {
					position: "bottom",
				},
			},
		},
	],
	xaxis: {
		type: "category",
		labels: {
			offsetY: 0.1
		}
	},
});

const selectedIndex = ref(null);

function handleDataSelection(e, chartContext, config) {
	if (!props.chart_config.map_filter) {
		return;
	}
	let toFilter = `${config.dataPointIndex} -${config.seriesIndex}`;
	if (toFilter !== selectedIndex.value) {
		mapStore.addLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`,
			props.chart_config.map_filter[0],
			props.chart_config.map_filter[1][config.dataPointIndex],
			props.map_config[0]
		);
		selectedIndex.value = toFilter;
	} else {
		mapStore.clearLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`,
			props.map_config[0]
		);
		selectedIndex.value = null;
	}
}

</script>

<template>
	<div v-if="activeChart === 'PolarAreaChart'">
		<apexchart
			type="polarArea"
			:options="chartOptions"
			:series="data"
			@dataPointSelection="handleDataSelection"
		/>
	</div>
</template>

<style lang="scss" scoped></style>
