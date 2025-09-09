<template>
  <v-chart class="chart" :option="option" autoresize />
  <CandlestickChart />
</template>

<script setup>
import { use } from 'echarts/core'
import { CandlestickChart, LineChart, BarChart } from 'echarts/charts'
import {
  LegendComponent,
  TooltipComponent,
  ToolboxComponent,
  BrushComponent,
  VisualMapComponent,
  GridComponent,
  DataZoomComponent
} from 'echarts/components'
import { CanvasRenderer } from 'echarts/renderers'
import VChart, { THEME_KEY } from 'vue-echarts';
import { ref, provide } from 'vue';

use([
  LegendComponent,
  TooltipComponent,
  ToolboxComponent,
  BrushComponent,
  VisualMapComponent,
  GridComponent,
  DataZoomComponent,
  CandlestickChart,
  LineChart,
  BarChart,
  CanvasRenderer
])

provide(THEME_KEY, 'dark');

const data = {
      categoryData: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
      values: [
        [23, 37, 30, 33, 38, 25, 35],
        [20, 34, 30, 33, 38, 25, 35],
        [36, 39, 30, 33, 38, 25, 35],
        [33, 38, 30, 33, 38, 25, 35],
        [38, 30, 33, 38, 25, 35],
        [38, 30, 33, 38, 25, 35],
        [38, 30, 33, 38, 25, 35]
      ],
      volumes: [
        [23, 37, 30, 33, 38, 25, 35],
        [20, 34, 30, 33, 38, 25, 35],
        [36, 39, 30, 33, 38, 25, 35],
        [33, 38, 30, 33, 38, 25, 35],
        [38, 30, 33, 38, 25, 35],
        [38, 30, 33, 38, 25, 35],
        [38, 30, 33, 38, 25, 35]
      ]
    }

const option = ref({
      animation: false,
      legend: {
        bottom: 10,
        left: 'center',
        data: ['Dow-Jones index']
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross'
        },
        borderWidth: 1,
        borderColor: '#ccc',
        padding: 10,
        textStyle: {
          color: '#000'
        },
        position: function (pos, params, el, elRect, size) {
          const obj = {
            top: 10
          };
          obj[['left', 'right'][+(pos[0] < size.viewSize[0] / 2)]] = 30;
          return obj;
        }
        // extraCssText: 'width: 170px'
      },
      axisPointer: {
        link: [
          {
            xAxisIndex: 'all'
          }
        ],
        label: {
          backgroundColor: '#777'
        }
      },
      toolbox: {
        feature: {
          dataZoom: {
            yAxisIndex: false
          },
          brush: {
            type: ['lineX', 'clear']
          }
        }
      },
      brush: {
        xAxisIndex: 'all',
        brushLink: 'all',
        outOfBrush: {
          colorAlpha: 0.1
        }
      },
      visualMap: {
        show: false,
        seriesIndex: 5,
        dimension: 2,
        pieces: [
          {
            value: 1,
            color: '#ec0000'
          },
          {
            value: -1,
            color: '#00da3c'
          }
        ]
      },
      grid: [
        {
          left: '10%',
          right: '8%',
          height: '50%'
        },
        {
          left: '10%',
          right: '8%',
          top: '63%',
          height: '16%'
        }
      ],
      xAxis: [
        {
          type: 'category',
          data: data.categoryData,
          boundaryGap: false,
          axisLine: { onZero: false },
          splitLine: { show: false },
          min: 'dataMin',
          max: 'dataMax',
          axisPointer: {
            z: 100
          }
        },
        {
          type: 'category',
          gridIndex: 1,
          data: data.categoryData,
          boundaryGap: false,
          axisLine: { onZero: false },
          axisTick: { show: false },
          splitLine: { show: false },
          axisLabel: { show: false },
          min: 'dataMin',
          max: 'dataMax'
        }
      ],
      yAxis: [
        {
          scale: true,
          splitArea: {
            show: true
          }
        },
        {
          scale: true,
          gridIndex: 1,
          splitNumber: 2,
          axisLabel: { show: false },
          axisLine: { show: false },
          axisTick: { show: false },
          splitLine: { show: false }
        }
      ],
      dataZoom: [
        {
          type: 'inside',
          xAxisIndex: [0, 1],
          start: 0,
          end: 100
        },
        {
          show: true,
          xAxisIndex: [0, 1],
          type: 'slider',
          top: '85%',
          start: 98,
          end: 100
        }
      ],
      series: [
        {
          name: 'Dow-Jones index',
          type: 'candlestick',
          data: data.values,
          itemStyle: {
            color: '#00da3c',
            color0: '#ec0000',
            borderColor: undefined,
            borderColor0: undefined
          }
        }
      ]
    });
</script>

<style scoped>
.chart {
  height: 100vh;
}
</style>
