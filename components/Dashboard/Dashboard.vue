<template>
  <v-container fluid fill-height class="pa-0">
    <v-row no-gutters align="center">
      <v-col cols="12" class="pb-2">
        <v-divider></v-divider>
      </v-col>
    </v-row>
    <v-row>
      <v-col :cols="bp.smAndDown ? 12 : 6">
        <v-row>
          <v-col cols="12" class="pt-0">
            <v-list class="transparent py-0">
              <v-list-item class="px-0 transparent py-0">
                <v-card-title class="py-0">
                  Weather Report
                </v-card-title>
                <v-spacer/>

              </v-list-item>
            </v-list>
          </v-col>
          <v-col cols="12" md="4" lg="3" sm="6" class="pa-1" v-for="n in 6" :key="n" v-if="loading">
            <v-skeleton-loader type="article, actions">

            </v-skeleton-loader>
          </v-col>
          <v-col cols="12" md="4" lg="4" sm="12" style="min-height: 320px" width="178px" v-if="!loading" class="pa-1"
                 v-for="(item,i) in items" :key="i">
            <v-card flat class="px-2 primary" height="100%" style="border-radius: 15px">
              <v-list-item class="white--text py-0">
                <v-list-item-content class="pb-0">
                  <div class="text-h6">
                    {{ item.name }}
                  </div>


                  <div class="text-overline">
                    {{ item.country.name }}
                  </div>
                </v-list-item-content>
              </v-list-item>
              <v-list-item align="center" class="align-self-center justify-center">

                <v-list-item-avatar
                  tile
                  size="80"
                  class="py-3"
                >
                  <v-img :src="selectIcon(item.weather.weather_condition)" contain>
                  </v-img>
                </v-list-item-avatar>
              </v-list-item>

              <div class="text-center">
                <h1
                  class="white--text text-center textColor"
                  :class="bp.mdAndUp ? 'mt-10': 'mb-2'"
                  style="font-size: 60px">
                  {{ Math.round(item.weather.temperature) }}<span>&#176;</span>
                </h1>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
      <v-col :cols="bp.smAndDown ? 12 : 6">
        <v-row>
          <v-col cols="12" class="pt-0">
            <v-list class="transparent py-0">
              <v-list-item class="px-0 transparent py-0">
                <v-card-title class="py-0">
                  Statistics
                </v-card-title>
                <v-spacer/>

                <v-menu offset-y>
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      small
                      rounded
                      v-bind="attrs"
                      v-on="on"
                      class="text-capitalize"
                    >
                      {{ selectedItem ? selectedItem : 'Select city' }}
                      <v-icon small right>
                        mdi-menu-down
                      </v-icon>
                    </v-btn>
                  </template>
                  <v-list>
                    <v-list-item
                      v-for="(item, index) in items"
                      :key="index"
                    >
                      <v-list-item-title>
                        <v-btn text small plain class="text-capitalize" @click="getWeatherReport(item)">
                          {{ item.name }}
                        </v-btn>
                      </v-list-item-title>
                    </v-list-item>
                  </v-list>
                </v-menu>

              </v-list-item>
            </v-list>
          </v-col>
          <v-col cols="12" v-show="stateLoading">
            <v-skeleton-loader
              v-show="stateLoading"
              type="article, actions"
            ></v-skeleton-loader>
          </v-col>
          <v-col cols="12">
            <charts
              v-if="!stateLoading"
              property-id="smoothLineChart"
              class-name="dashboardLineChart"
              :axis-data="temperature.hour"
              :chart-data="temperature.value"
              card-title="Temperature (Last 24 hours)"
              :report-date="report_date"
            />
          </v-col>
          <v-col cols="12">
            <charts
              v-if="!stateLoading"
              property-id="normalLineChart"
              class-name="dashboardLineChart"
              :axis-data="wind_speed.hour"
              :chart-data="wind_speed.value"
              card-title="Wind (Last 24 hours)"
              smooth="smooth"
              :report-date="report_date"
            />
          </v-col>
          <v-col cols="12">
            <bar-chart-component
              v-if="!stateLoading"
              chart-title="Humidity (Last 24 hours)"
              property-id="dashboardAccChart"
              :axis-data="humidity.hour"
              :chart-data="humidity.value"
              high-or-low="low"
              different-value="-13%"
              class-name="dashboardBarChart"
              :report-date="report_date"
            />
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import TitleCard from '../Common/TitleCard.vue'
import BarChartComponent from '../Statistics/ChartComponent/BarChartComponent.vue'
import {mapGetters} from "vuex";
import clear from 'static/icons/clear.png'
import sunny from 'static/icons/sunny.png'
import rainy from 'static/icons/rain.png'
import cloudy from 'static/icons/cloudy.png'
import Charts from "~/components/Statistics/Temparature/Charts.vue";

export default {
  name: 'Dashboard',
  components: {
    Charts,
    TitleCard,
    BarChartComponent,
  },
  data() {
    return {
      loading: false,
      stateLoading: false,
      selectedItem: null,
      date: null,
      menu: false,
      sunny, rainy, cloudy, clear,
      show: false,
      items: [],
      data: {
        dates: ['00', '02', '04', '06', '08', '10', '12', '14', '16', '18', '20', '22', '24'],
        regBarData: [150, 230, 224, 218, 135, 147, 150, 230, 224, 218, 135, 147]
      },
      humidity : {
        hour: [],
        value: []
      },
      temperature : {
        hour: [],
        value: []
      },
      wind_speed : {
        hour: [],
        value: []
      },
      report_date: null
    }
  },

  created() {
    this.initialize()
  },

  methods: {
    close() {
      this.menu = false
    },
    initialize() {
      this.loading = true
      this.$axios.get('weather')
        .then((response) => {
          this.items = response.data.data
          this.getWeatherReport(this.items[0])
          this.show = true
        })
        .catch((error) => {
        })
        .finally(() => {
          this.loading = false
        })
    },
    selectIcon(condition) {
      switch (condition) {
        case 'Clear':
          return clear;
        case 'Rainy':
          return rainy;
        case 'Clouds':
          return cloudy;

      }
    },
    getWeatherReport(item) {
      this.stateLoading = true
      this.selectedItem = item.name
      this.$axios.get('/history/' + item.id)
        .then((response) => {
          console.log(response)
          this.wind_speed = Object.assign({},response.data.data.wind_speed)
          this.temperature = Object.assign({},response.data.data.temperature)
          this.humidity = Object.assign({},response.data.data.humidity)
          this.report_date = response.data.data.report_date
        })
        .catch((error) => {
          this.$toast.error(error.response.data.message)
        })
        .finally(() => {
          this.stateLoading = false
        })
    }

  },
}
</script>

<style scoped>
.radius {
  border-radius: 7px;
}

.dashboardLineChart {
  width: 700px;
  height: 250px;
}

.textColor {
  font-size: 72px;
  background: -webkit-linear-gradient(#efeeef, #8c79b6);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
