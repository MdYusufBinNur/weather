<template>
  <v-container fluid class="pa-0 ma-0">
    <v-card flat color="white">
      <v-row>
        <v-col cols="9">
          <v-card-title>{{ cardTitle}}</v-card-title>
        </v-col>
        <v-col cols="3" align="right">
          <v-btn depressed right x-small rounded class="mt-5 mr-5">
            {{ reportDate ? reportDate : '' }}
          </v-btn>
        </v-col>
      </v-row>
      <v-row align="center" no-gutters class="d-flex justify-center">
        <div :id="propertyId" :class="className"></div>
        <v-card-text :class="'title ' + iconColor+'--text'">
          {{ chartDesc ? chartDesc : '' }}
        </v-card-text>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "Charts",
  props: {
    reportDate: {
      req: false,
      type: String
    },
    cardTitle: {
      required : false,
      type:String
    },
    chartDesc: {
      required: false,
      type: String
    },
    propertyId: {
      required: false,
      type: String
    },
    chartData: {
      required: false,
      type: Array,
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
    ,
    smooth: {
      required : false,
      type: String
    }


  },
  data() {
    return {
      iconName: 'mdi-chevron-down-circle-outline',
      iconColor: '',
      temp: {
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
            type: 'line',
            smooth: false
          }
        ]
      }
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

      // if (this.highOrLow === 'high') {
      //   this.iconName = 'mdi-chevron-up-circle-outline'
      //   this.iconColor = 'success'
      // } else {
      //   this.iconName = 'mdi-chevron-down-circle-outline'
      //   this.iconColor = 'error'
      // }
      // Start rendering
      this.temp.xAxis.data = this.axisData
      this.temp.series[0].data = this.chartData
      if (this.smooth === 'smooth') {
        this.temp.series[0].smooth = true
      }

      myChart.setOption(this.temp)
      myChart.resize(this.size)

    },
  },
}
</script>

<style scoped>
.dashboardLineChart {
  width: 700px;
  height: 250px;
}


</style>
