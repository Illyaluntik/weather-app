<template>
  <div class="detailed-forecast">
    <h1 v-if="weatherInfo && !serverError" class="title">Weather in {{ weatherInfo?.location.name }}</h1>
    <span v-if="serverError" class="server-error">No matching location found</span>
    <div class="hourly-forecast">
      <ForecastHourlyCard v-for="hour of weatherInfo?.forecast.forecastday[0].hour.slice(new Date().getHours())" :key="hour" :weather="hour"/>
      <ForecastHourlyCard v-for="hour of weatherInfo?.forecast.forecastday[1].hour.slice(0, new Date().getHours())" :key="hour" :weather="hour"/>
    </div>
    <div class="daily-forecast">
      <ForecastDailyCard v-for="day of weatherInfo?.forecast.forecastday" :key="day" class="day" :weather="day"/>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios';
import ForecastHourlyCard from '@/components/ForecastHourlyCard.vue'
import ForecastDailyCard from '@/components/ForecastDailyCard.vue'

export default defineComponent({
  name: 'Detailed Forecast',
  components: {
    ForecastHourlyCard,
    ForecastDailyCard
  },
  data() {
    return {
      city: 'Minsk',
      weatherInfo: null,
      serverError: false
    }
  },
  watch: {
    '$route.params': function(to: any) {
      if (this.$route.name != 'detailed-forecast')
        return;

      this.city = to.city;
      this.getWeather();
    }
  },
  methods: {
    async getWeather() {
      await axios
        .get(`http://api.weatherapi.com/v1/forecast.json?key=9c20b731f98a4e3e8b1114012210710&q=${this.city}&days=10&aqi=no&alerts=no`)
        .then(
          res => {
            this.weatherInfo = res.data
            this.serverError = false;
          })
        .catch(err => {
          console.error(err);
          this.weatherInfo = null;
          this.serverError = true
        });
    },
  },
  mounted() {
    this.city = this.$route.params.city as string;
    this.getWeather();
  }
})
</script>

<style lang="less" scoped>
.detailed-forecast {
  max-width: 1200px;
  margin: 0 auto;
  .title, .server-error {
    font-size: 40px;
    margin-top: 50px;
  }
  .server-error {
    display: inline-block;
  }
}
.hourly-forecast {
  display: flex;
  margin-top: 50px;
  padding: 30px 0;
  width: 100%;
  overflow-x: auto;
}
</style>