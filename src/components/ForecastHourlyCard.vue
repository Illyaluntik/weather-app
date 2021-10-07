<template>
  <div class="forecast-hourly-card">
    <span class="time">{{ weatherDate }}</span><br>
    <img :src="weather.condition.icon" alt="weather-condition" class="icon">
    <span v-if="weather.chance_of_rain > 0" class="chance-of-rain">{{ weather.chance_of_rain }}%</span>
    <span class="weather-condition">{{ weather.condition.text }}</span>
    <span class="temperature">{{ weather.temp_c }} &#8451;</span>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    weather: { type: Object, required: true }
  },
  computed: {
    weatherDate() {
      if (new Date(this.weather.time).getHours() == new Date().getHours())
        return 'Now';

      return new Date(this.weather.time).getHours() + ':00';
    }
  }
})
</script>

<style lang="less" scoped>
.forecast-hourly-card {
  box-shadow: 0 2px 10px rgba(0, 0, 0, .15);
  border-radius: 8px;
  margin: 0 20px;
  padding: 10px 20px;
  min-width: 160px;

  > span {
    display: block;
  }
  .time {
    font-weight: 600;
  }
  .weather-condition {
    line-height: 20px;
    margin-top: 10px;
  }
  .temperature {
    font-weight: 700;
    margin-top: 5px;
  }
  .icon {
    margin: 0 auto;
  }
}
</style>