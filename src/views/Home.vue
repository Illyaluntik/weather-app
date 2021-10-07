<template>
  <div class="cities">
    <div v-for="city of cities" :key="city" class="city-card" :class="{ 'selected': city == selectedCity }" @click="getWeather(city)">
      <span class="city-name">{{ city }}</span>
    </div>
  </div>
  <div v-show="weatherInfo" class="forecast">
    <h1 class="title">Forecast for {{ selectedCity }}:</h1>
    <ForecastDailyCard v-for="day of weatherInfo?.forecast.forecastday" :key="day" class="day" :weather="day"/>
    <router-link :to="{ name: 'detailed-forecast', params: { city: selectedCity }}">
      <button>Detailed Forecast</button>
    </router-link>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import ForecastDailyCard from '@/components/ForecastDailyCard.vue'

export default defineComponent({
  name: 'Home',
  components: {
    ForecastDailyCard
  },
  data() {
    return {
      cities: ['Minsk', 'Moscow', 'Bratislava'],
      selectedCity: 'Minsk',
      weatherInfo: null
    }
  },
  watch: {
    selectedCity: function() {
      window.localStorage.setItem('DEFAULT_CITY', this.selectedCity);
    }
  },
  methods: {
    async getWeather(city:string) {
      this.selectedCity = city;

      await axios
        .get(`http://api.weatherapi.com/v1/forecast.json?key=9c20b731f98a4e3e8b1114012210710&q=${city}&days=3&aqi=no&alerts=no`)
        .then(res => (this.weatherInfo = res.data))
        .catch(err => console.error(err));
    }
  },
  mounted() {
    this.selectedCity = window.localStorage.getItem('DEFAULT_CITY') || this.cities[0];
    this.getWeather(this.selectedCity);
  }
});
</script>

<style lang="less" scoped>
.cities {
  display: flex;
  justify-content: space-between;
  max-width: 1200px;
  margin: 50px auto 0;

  .city-card {
    width: 25%;
    padding: 15px 20px;
    border-radius: 8px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, .2);
    cursor: pointer;
    transition: opacity ease 0.5s;
    opacity: 0.25;

    &:hover { opacity: 1; }
    &.selected { opacity: 1; }
    .city-name { font-size: 30px; }
  }
}
.forecast {
  margin-top: 50px;

  .title { font-size: 40px; }
  button {
    background: #7e8588;
    color: #ffffff;
    border: none;
    outline: none;
    box-shadow: 0 2px 5px rgba(0, 0, 0, .2);
    border-radius: 8px;
    padding: 10px 20px;
    transition: color, background ease 0.2s;
    margin-top: 20px;

    &:hover {
      background: #e4e8eb;
      color: #7e8588;
    }
  }
}
</style>