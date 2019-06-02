<template>
  <div id="app">
    <HelloWorld msg="Weather"/>
    <div class="form-group">
      <input type="text" v-model="search" placeholder="Search City or Zip">
      <button class="btn btn-default" v-on:click="searchweather()">Search</button>
    </div>
    <div>
      <b-card v-if="info">
        <h2 class="text-left">{{info}}</h2>
        <b-card-text>
          <p class="text-left">{{myDate}}</p>
          <p class="text-left">{{max}}{{min}}</p>
          <img :src="path+images+'.png'" alt="">
          <p>{{temp}}</p>
          <p>{{main}}</p>
        </b-card-text>
      </b-card>
      <b-card v-if="info">
        <b-card-text class="text-left">CURRENT DETAILS</b-card-text>
        <P class="text-left">Humidity: {{humidity}}%</p>
        <p class="text-left">Wind: {{speed}} Km/h</p>
        <p class="text-left">pressure: {{pressure}} mBar</p>
        <p class="text-left">Chance of rain: {{all}}%</p>
        <!-- <b-link href="#" class="card-link">Another link</b-link> -->
      </b-card>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  components: {
    HelloWorld
  },
  data() {
    return {
      info: null,
      object:[],
      search: "",
      max: null,
      min: null,
      temp: null,
      images: null,
      main: null,
      humidity: null,
      pressureL: null,
      speed: null,
      all:null,
      path:'http://openweathermap.org/img/w/',
      myDate : new Date().toISOString().slice(0,10)
    };
  },
  methods: {
    searchweather() {
      axios
        .get(
          "https://api.openweathermap.org/data/2.5/weather?q=" +
            this.search +
            "&appid=d5302cb77cfc4b3a7dde03e7ea25272b"
        )
        .then(response => {
          this.info = response.data.name;
          this.max =
            "Max: " + (response.data.main.temp_max - 273.15).toFixed(0) + "°";
          this.min =
            ", Min: " + (response.data.main.temp_min - 273.15).toFixed(0) + "°";
          this.temp = (response.data.main.temp - 273.15).toFixed(0) + "°";
          this.images = response.data.weather[0].icon;
          this.main = response.data.weather[0].main;
          this.humidity = response.data.main.humidity;
          this.speed = response.data.wind.speed;
          this.pressure = response.data.main.pressure;
          this.all = response.data.clouds.all;
        });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
