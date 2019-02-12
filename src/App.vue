<template>
  <div class="container">
    <div class="search">
      <input type="text" placeholder="Search">
    </div>
    <div class="wind">
      <icon-wind></icon-wind>
      <p class="label">Wind speed</p>
      <p class="result">{{wind}} meter/sec</p>
    </div>
    <div class="pressure">
      <icon-pressure></icon-pressure>
      <p class="label">Pressure</p>
      <p class="result">{{pressure}} hPa</p>
    </div>
    <div class="humidity">
      <icon-humidity></icon-humidity>
      <p class="label">Humidity</p>
      <p class="result">{{humidity}} %</p>
    </div>
    <div class="sun">
      <div class="sunrise">
        <div class="label">Sunrise</div>
        <div class="result">
          {{sunrise}}
          <icon-sunrise></icon-sunrise>
        </div>
      </div>
      <div class="sunset">
        <div class="result">
          {{sunset}}
          <icon-sunset></icon-sunset>
        </div>
        <div class="label">Sunset</div>
      </div>
    </div>
    <div class="main">
      <div class="city">
        {{city}}, {{country}}
        <span>Change location</span>
      </div>
      <div class="icon">
        <component v-bind:is="icon"></component>
      </div>
      <div class="temp">{{temp}}°C</div>
      <div class="description">{{description}}</div>
    </div>
  </div>
</template>

<script>
import BrokenClouds from "./assets/SVGicons/BrokenClouds";
import Wind from "./assets/SVGicons/Wind";
import Humidity from "./assets/SVGicons/Humidity";
import Pressure from "./assets/SVGicons/Pressure";
import Sunrise from "./assets/SVGicons/Sunrise";
import Sunset from "./assets/SVGicons/Sunset";
import ClearSky from "./assets/SVGicons/ClearSky";

export default {
  components: {
    "icon-brokenclouds": BrokenClouds,
    "icon-wind": Wind,
    "icon-humidity": Humidity,
    "icon-pressure": Pressure,
    "icon-sunrise": Sunrise,
    "icon-sunset": Sunset,
    "clear-sky": ClearSky
  },

  data() {
    return {
      temp: "",
      city: "",
      country: "",
      wind: "",
      pressure: "",
      humidity: "",
      sunrise: "",
      sunset: "",
      description: "",
      icon: ""
    };
  },

  beforeMount() {
    this.getWeather();
  },

  methods: {
    getWeather: function() {
      const axios = require("axios");
      let url =
        "http://api.openweathermap.org/data/2.5/weather?q=Rijeka,hr&units=metric&APPID=20b1e607d84c252bf491062a482382b0";
      axios
        .get(url)
        .then(response => {
          this.temp = response.data.main.temp;
          this.humidity = response.data.main.humidity;
          this.pressure = response.data.main.pressure;
          this.city = response.data.name;
          this.country = response.data.sys.country;
          this.wind = response.data.wind.speed;
          this.sunrise = new Date(response.data.sys.sunrise * 1000)
            .toLocaleTimeString("hr-HR")
            .slice(0, 5);
          this.sunset = new Date(response.data.sys.sunset * 1000)
            .toLocaleTimeString("hr-HR")
            .slice(0, 5);
          this.description = response.data.weather[0].description;
          this.icon = (() => {
            let tempString = this.description
              .replace(/\s+/g, "-")
              .toLowerCase();

            return tempString;
          })();
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>

<style lang="scss">
@import "./scss/App.scss";
</style>