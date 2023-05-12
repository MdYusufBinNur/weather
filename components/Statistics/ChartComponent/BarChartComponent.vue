<template>
  <v-container fluid class="pa-0 ma-0">
    <v-card flat color="white">
      <v-row>
        <v-col cols="9">
          <v-card-title>Humidity (Last 24 hours)</v-card-title>
        </v-col>
        <v-col cols="3" align="right">
          <v-btn depressed right x-small rounded class="mt-5 mr-5">
            All
          </v-btn>
        </v-col>
      </v-row>
      <v-row align="center" no-gutters class="d-flex justify-center">
        <div id="propertyId123" :class="className"></div>
        <v-card-text :class="'title ' + iconColor+'--text'">
          <v-icon :color="iconColor" small>
            mdi-chevron-up-circle-outline
          </v-icon>
          {{ chartDesc ? chartDesc : '' }}
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
    highOrLow: {
      required: false,
      type: String
    },
    differentValue: {
      required: false,
      type: String
    }

  },
  data() {
    return {
      weekly_btn: true,
      iconName: 'mdi-chevron-down-circle-outline',
      iconColor: '',
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
        color: ['#4462d3', '#8096de', '#8096de'],
        grid: {
          left: '10%',
          right: '10%',
          bottom: '5%',
          containLabel: true,
        },
        xAxis: {
          type: 'category',
          data: []
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            data: [],
            type: 'bar',
            showBackground: true,
            backgroundStyle: {
              color: '#a5b7ef'
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
        document.getElementById('propertyId123')
      )

      if (this.highOrLow === 'high') {
        this.iconName = 'mdi-chevron-up-circle-outline'
        this.iconColor = 'success'
      } else {
        this.iconName = 'mdi-chevron-down-circle-outline'
        this.iconColor = 'error'
      }
      // Start rendering
      this.barChartOption.xAxis.data = this.axisData
      this.barChartOption.series[0].data = this.chartData
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
