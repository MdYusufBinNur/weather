<template>
  <v-container fluid class="pa-0 ma-0">
    <v-card flat color="white">
      <v-row>
        <v-col cols="9">
          <v-card-title>{{ chartTitle }}</v-card-title>
        </v-col>
        <v-col cols="3" align="right">
          <v-btn depressed right x-small rounded class="mt-5 mr-5">
            All
          </v-btn>
        </v-col>
      </v-row>
      <v-row align="center" no-gutters class="d-flex justify-center">
        <div :id="propertyId" :class="className"></div>
        <v-card-text class="caption">
          {{ chartDesc }}
        </v-card-text>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'BarChartComponent',
  props: {
    chartTitle: {
      required: true,
      type: String,
    },
    chartDesc: {
      required: true,
      type: String,
    },
    axisData: {
      required: false,
      type: Array,
    },
    chartData: {
      required: false,
      type: Array,
    },
    propertyId: {
      required: true,
      type: String,
    },
    className: {
      required: false,
      type: String,
    },
  },
  data() {
    return {
      weekly_btn: true,
      barChartOption: {
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            // Use axis to trigger tooltip
            type: 'shadow', // 'shadow' as default; can also be 'line' or 'shadow'
          },
        },
        legend: {
          right: 'right',
        },
        color: ['#2C3040', '#6D7079', '#7D85A1'],
        grid: {
          left: '10%',
          right: '10%',
          bottom: '5%',
          containLabel: true,
        },
        xAxis: {
          type: 'category',
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            data: [120, 200, 150, 80, 70, 110, 130],
            type: 'bar',
            showBackground: true,
            backgroundStyle: {
              color: 'rgba(180, 180, 180, 0.2)'
            }
          }
        ]
      },
      size: {
        width: 'auto',
        height: 'auto',
      },
    }
  },
  mounted() {
    this.initCharts()
  },
  methods: {
    initCharts() {
      const myChart = this.$echarts.init(
        document.getElementById(this.propertyId)
      )
      // Start rendering
      this.barChartOption.xAxis.data = this.axisData
      this.barChartOption.series = this.chartData
      myChart.setOption(this.barChartOption)
      myChart.resize(this.size)
    },
  },
}
</script>

<style scoped>
.coupleCharts {
  width: 700px;
  height: 300px;
}
.weddingCharts {
  width: 500px;
  height: 250px;
}
.dashboardBarChart {
  width: 700px;
  height: 250px;
}
</style>
