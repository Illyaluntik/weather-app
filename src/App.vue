<template>
  <header>
    <router-link to="/">
      <button class="to-home">Home</button>
    </router-link>
    <div class="search-input">
      <input type="text" placeholder="Enter city" @keyup.enter="searchCity" v-model="searchString">
      <button @click="searchCity">Search</button>
    </div>
  </header>
  <router-view/>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      searchString: ''
    }
  },
  methods: {
    async searchCity() {
      if (!this.searchString)
        return;
      await this.$router.push({ name: 'detailed-forecast', params: { city: this.searchString } });
      this.searchString = '';
    },
  }
})
</script>


<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
header {
  display: flex;
  justify-content: space-between;
  padding: 20px 50px;
  box-shadow: 0 0px 10px rgba(0, 0, 0, .2);
  // background: #e2e7ec;
  background: #eaeff3;

  button {
    background: #ffffff;
    border: none;
    outline: none;
    box-shadow: 0 2px 5px rgba(0, 0, 0, .2);
    border-radius: 8px;
    padding: 10px 20px;
    transition: color, background ease 0.2s;

    &:hover {
      background: #757779;
      color: #ffffff;
    }
  }
  .search-input {
    input {
      width: 500px;
      height: 100%;
      border: none;
      outline: none;
      background: #ffffff;
      box-shadow: 0 2px 5px rgba(0, 0, 0, .2);
      border-radius: 8px;
      margin-right: 15px;
    }
  }
}
</style>