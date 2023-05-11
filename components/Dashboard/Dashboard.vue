<template>
  <v-container fluid fill-height class="pa-0">
    <v-row no-gutters align="center">
      <v-col cols="12" class="pb-2">
        <v-divider></v-divider>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="6">
        <v-row>
          <v-col cols="12" md="4" lg="4" sm="6" class="pa-1" v-for="n in 6" :key="n" v-if="loading">
            <v-skeleton-loader type="article, actions">

            </v-skeleton-loader>
          </v-col>
          <v-col cols="12" md="4" lg="4" sm="5" v-if="!loading" class="pa-1" v-for="(item,i) in items" :key="i">
            <v-card flat class="px-2 primary" height="266px" width="178px" style="border-radius: 15px">
              <v-list-item class="white--text">
                <v-list-item-content>
                  <div class="text-h5">
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
                  size="100"
                >
                  <v-img :src="selectIcon(item.weather.weather_condition)" contain>
                  </v-img>
                </v-list-item-avatar>
              </v-list-item>

              <div class="text-center">
                <h1 class="white--text text-center">
                  {{ Math.round(item.weather.temperature) }}<span>&#176;</span>
                </h1>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="6">
        <v-row>
          <v-col cols="12">
            <BarChartComponent v-if="show" property-id="testBarChat" chart-desc="Test" chart-title="Wind Speed" />
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import TitleCard from '../Common/TitleCard.vue'
import PieChartComponent from '../Statistics/ChartComponent/PieChartComponent.vue'
import BarChartComponent from '../Statistics/ChartComponent/BarChartComponent.vue'
import {mapGetters} from "vuex";
import clear from 'static/icons/clear.png'
import sunny from 'static/icons/sunny.png'
import rainy from 'static/icons/rain.png'
import cloudy from 'static/icons/cloudy.png'

export default {
  name: 'Dashboard',
  components: {
    TitleCard,
    PieChartComponent,
    BarChartComponent,
  },
  data() {
    return {
      loading: false,
      date: null,
      menu: false,
      sunny, rainy, cloudy, clear,
      weddingNumbers: [
        {
          index: 0,
          text: ' User',
          type: 'user',
          value: 0,
          icon: 'mdi-account'
        },
        {
          index: 1,
          text: ' Doctors',
          type: 'doctor',
          value: 0,
          icon: 'mdi-account-star'
        },
        {
          index: 2,
          text: ' Phlebotomist',
          type: 'phlebotomist',
          value: 0,
          icon: 'mdi-run'
        },
        {
          index: 3,
          text: 'Lab',
          type: 'labs',
          value: 0,
          icon: 'mdi-trending-up'
        },

      ],
      show: false,
      items: []
    }
  },
  computed: {
    ...mapGetters({
    })
  },
  watch: {
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
          this.show = true
        })
        .catch((error) => {
        })
        .finally(() => {
          this.loading = false
        })
    },
    setBarChartData() {
      this.regBarAxis = this.data.months
      this.regBarData[0].data = this.data.monthsBaseUser
      this.show = true
    },
    selectIcon(condition)
    {
      switch (condition) {
        case 'Clear':
          return clear;
        case 'Rainy':
          return rainy;
        case 'Clouds':
          return cloudy;

      }
    }
  },
}
</script>

<style scoped>
.radius {
  border-radius: 7px;
}

</style>
