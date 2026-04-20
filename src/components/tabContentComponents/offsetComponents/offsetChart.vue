<template>
  <div id="chart" ref="chartRef" class="uk-margin-large"></div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import * as echarts from 'echarts'

// Define the type for ECharts option for better DX
type EChartsOption = echarts.EChartsOption

const chartRef = ref<HTMLElement | null>(null)
let chartInstance: echarts.ECharts | null = null

function func(x) {
  x /= 10
  return Math.sin(x) * Math.cos(x * 2 + 1) * Math.sin(x * 3 + 2) * 50
}

function generateData() {
  const data = []
  for (let i = -200; i <= 200; i += 0.1) {
    data.push([i, func(i)])
  }
  return data
}

const initChart = () => {
  if (chartRef.value) {
    chartInstance = echarts.init(chartRef.value)

    const option: EChartsOption = {
      animation: false,
      grid: {
        top: 40,
        left: 50,
        right: 40,
        bottom: 50,
      },
      xAxis: {
        name: 'x',
        minorTick: {
          show: true,
        },
        minorSplitLine: {
          show: true,
        },
      },
      yAxis: {
        name: 'y',
        min: -100,
        max: 100,
        minorTick: {
          show: true,
        },
        minorSplitLine: {
          show: true,
        },
      },
      dataZoom: [
        {
          show: true,
          type: 'inside',
          filterMode: 'none',
          xAxisIndex: [0],
          startValue: -20,
          endValue: 20,
        },
        {
          show: true,
          type: 'inside',
          filterMode: 'none',
          yAxisIndex: [0],
          startValue: -20,
          endValue: 20,
        },
      ],
      series: [
        {
          type: 'line',
          showSymbol: false,
          clip: true,
          data: generateData(),
        },
      ],
      autoresize: true,
    }

    chartInstance.setOption(option)
  }
}

// Handle responsiveness
const handleResize = () => {
  chartInstance?.resize()
}

onMounted(async () => {
  await nextTick()

  initChart()
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  chartInstance?.dispose() // Clean up to prevent memory leaks
})
</script>

<style scoped>
#chart {
  width: 640px;
  /* min-width: 400px;
  max-width: 1200px; */
  height: 480px;
  margin: 20px 20px 20px 60px;
}
</style>
