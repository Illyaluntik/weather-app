<template>
  <div class="forecast-daily-card">
    <span class="date">{{ new Date(weather.date).setHours(0,0,0,0) == new Date().setHours(0,0,0,0) ? 'Today' : weather.date }}</span>
    <span class="temperature">{{ weather.day.avgtemp_c }} &#8451;</span>
    <span class="condition">{{ weather.day.condition.text }}</span>
    <img :src="weather.day.condition.icon" alt="weather-condition" class="icon">
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    weather: { type: Object, required: true }
  }
})
</script>

<style lang="less" scoped>
.forecast-daily-card {
  margin-top: 25px;
  font-size: 20px;

  > span {
    margin-right: 15px;
    vertical-align: middle;
  }
  .icon {
    vertical-align: middle;
    display: inline;
  }
  .temperature { font-weight: 700; }
}
</style>