<template>
  <div id="chart" ref="chartRef"></div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import * as echarts from 'echarts'
import 'echarts-gl'

// Define the type for ECharts option for better DX
type EChartsOption = echarts.EChartsOption

const chartRef = ref<HTMLElement | null>(null)
let chartInstance: echarts.ECharts | null = null

const initChart = () => {
  if (chartRef.value) {
    chartInstance = echarts.init(chartRef.value)

    const option: EChartsOption = {
      tooltip: {},
      backgroundColor: '#fff',
      visualMap: {
        show: false,
        dimension: 2,
        min: -1,
        max: 1,
        inRange: {
          color: [
            '#313695',
            '#4575b4',
            '#74add1',
            '#abd9e9',
            '#e0f3f8',
            '#ffffbf',
            '#fee090',
            '#fdae61',
            '#f46d43',
            '#d73027',
            '#a50026',
          ],
        },
      },
      xAxis3D: {
        type: 'value',
      },
      yAxis3D: {
        type: 'value',
      },
      zAxis3D: {
        type: 'value',
      },
      grid3D: {
        viewControl: {
          // projection: 'orthographic'
        },
      },
      series: [
        {
          type: 'surface',
          wireframe: {
            // show: false
          },
          equation: {
            x: {
              step: 0.05,
            },
            y: {
              step: 0.05,
            },
            z: function (x, y) {
              if (Math.abs(x) < 0.1 && Math.abs(y) < 0.1) {
                return '-'
              }
              return Math.sin(x * Math.PI) * Math.sin(y * Math.PI)
            },
          },
        },
      ],
    }

    chartInstance.setOption(option)
  }
}

// Handle responsiveness
const handleResize = () => {
  chartInstance?.resize()
}

onMounted(() => {
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
