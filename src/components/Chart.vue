<template>
  <div id="chart" ref="chartRef"></div>
</template>

<script setup>
import { ref, onMounted, toRef, watch } from "vue";
import { init } from "echarts";

const props = defineProps({
  options: Object,
});

const chartOptions = toRef(props, "options");
const chartRef = ref(null);
let chart;

watch(chartOptions, (newVal, oldVal) => {
  chart.setOption(newVal);
});

onMounted(() => {
  chart = init(chartRef.value);
  console.log("chart ref", props.options);
  chart.setOption(props.options);
});
</script>

<style lang="scss" scoped>
#chart {
  height: 90%;
  width: 90%;
}
</style>
