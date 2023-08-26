<template>
  <div class="home">
    <header>Chart-er</header>
    <div class="container">
      <div class="canvas">
        <Chart :options="chartOptions" />
      </div>
      <div class="sidebar">
        <Button text="Chart Config" @click="openOverlay('chart-config')" />
        <Button text="X Axis" @click="openOverlay('x-axis-config')" />
        <Button text="Y Axis" @click="openOverlay('y-axis-config')" />
        <Button text="Data Points" @click="openOverlay('series-config')" />
        <ConfigOverlay v-if="overlayOpen">
          <ChartConfig
            v-if="currentOverlay == 'chart-config'"
            @saveConfig="updateConfig"
            @closeOverlay="resetOverlays"
            @saveOptions="updateOptions"
            :currentConfig="chartOptions"
          />
          <SeriesConfig
            v-if="currentOverlay == 'series-config'"
            @saveSeries="updateSeries"
            @closeOverlay="resetOverlays"
            :currentSeries="chartOptions.series"
          />
          <xAxisConfig
            v-if="currentOverlay == 'x-axis-config'"
            :currentXAxis="chartOptions.xAxis"
            @saveXAxis="saveXAxis"
            @closeOverlay="resetOverlays"
          />
          <yAxisConfig
            v-if="currentOverlay == 'y-axis-config'"
            :currentYAxis="chartOptions.yAxis"
            @saveYAxis="saveYAxis"
            @closeOverlay="resetOverlays"
          />
        </ConfigOverlay>
      </div>
    </div>
  </div>
</template>

<script setup>
import Button from "@/components/Button.vue";
import ConfigOverlay from "../components/ConfigOverlay.vue";
import Chart from "../components/Chart.vue";
import ChartConfig from "../components/ChartConfig.vue";
import SeriesConfig from "../components/SeriesConfig.vue";
import xAxisConfig from "../components/xAxisConfig.vue";
import yAxisConfig from "../components/yAxisConfig.vue";

import { ref } from "vue";

const overlayOpen = ref(false);
const currentOverlay = ref("");
const openOverlay = (overlay) => {
  overlayOpen.value = true;
  currentOverlay.value = overlay;
};

const resetOverlays = () => {
  overlayOpen.value = false;
  currentOverlay.value = "";
};

const updateSeries = (series) => {
  console.log("updating series", series);
  chartOptions.value = {
    ...chartOptions.value,
    series: series,
  };
  resetOverlays();
};

const saveXAxis = (xAxis) => {
  console.log("saving x axis", xAxis);
  chartOptions.value = {
    ...chartOptions.value,
    xAxis: xAxis,
  };
  resetOverlays();
};

const saveYAxis = (yAxis) => {
  console.log("saving y axis", yAxis);
  chartOptions.value = {
    ...chartOptions.value,
    yAxis: yAxis,
  };
  resetOverlays();
};

const updateConfig = (config) => {
  console.log("updating config", config);
  // let temp = JSON.parse(JSON.stringify(chartOptions.value));
  // console.log("temp", temp);
  chartOptions.value = {
    ...chartOptions.value,
    title: {
      text: config.title,
      show: config.showTitle,
    },
    legend: {
      show: config.showLegend,
    },
    grid: {
      show: config.showGrid,
      tooltip: {
        show: config.showGridTooltip,
      },
    },
    tooltip: {
      show: config.showTooltip,
    },
  };

  // console.log("chart options");

  resetOverlays();
};

const updateOptions = (options) => {
  console.log("updating options", options);
  chartOptions.value = JSON.parse(options);
  resetOverlays();
};

const chartOptions = ref({
  title: {
    text: "",
    show: false,
  },
  legend: {
    show: false,
  },
  grid: {
    show: false,
    tooltip: {
      show: false,
    },
  },
  tooltip: {
    show: false,
  },
  xAxis: {},
  yAxis: {},
  series: [],
});
</script>

<style lang="scss" scoped>
.home {
  height: 100%;
  width: 100%;
}

header {
  height: 60px;
  width: 100%;

  background: #ececec;
  display: flex;
  justify-content: center;
  align-items: center;

  font-size: 24px;
}

.container {
  height: calc(100% - 60px);
  width: 100%;

  display: flex;

  .canvas {
    height: 100%;
    width: 100%;
    background: #fff;

    display: flex;
    justify-content: center;
    align-items: center;
  }

  .sidebar {
    height: 100%;
    width: 400px;
    background: #ffffff;
    border-left: 2px solid #ececec;

    display: flex;
    // justify-content: center;
    align-items: center;
    flex-direction: column;
  }
}
</style>
