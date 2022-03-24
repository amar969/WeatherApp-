<template>
  <div
    id="app"
  >
    <main>
      <div class="search-bar">
        <img src="https://img.icons8.com/ios-filled/50/000000/marker.png" />
        <input
          type="text"
          class="search-box"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
        <img
          class="magnify-icon"
          src="https://img.icons8.com/ios-filled/50/000000/search--v1.png"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp) }}°C
            <img
              :src="
                'http://openweathermap.org/img/wn/' +
                weather.weather[0].icon +
                '.png'"/>
          </div>
          <div></div>
          <div class="pressure">
            <h6>Pressure</h6>
            {{ weather.main.pressure }} hpa
          </div>
          <div class="humidity">
            <h6>Humidity</h6>
            {{ weather.main.humidity }} %
          </div>
          <div class="sun">
            <h6>Sunrise</h6>
            <p>{{ convertTime(weather.sys.sunrise) }} AM</p>
          </div>
          <div class="sun">
            <h6>Sunset</h6>
            <p>{{ convertTime(weather.sys.sunset) }} PM</p>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "09cfa5ea3bfe583999b646646dc2a656",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      weatherfor7: {},
      latitude: "",
      longitude: "",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },

    fetchWeather7days(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}forecast?q=${this.query}&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults1);
      }
    },

    setResults1(results1) {
      this.weatherfor7 = results1;
      },

    getCurrentLocation(){
      if(navigator.geolocation){
      navigator.geolocation.getCurrentPosition( position => {
        console.log(position.coords.latitude);
        console.log(position.coords.longitude);
        this.latitude = position.coords.latitude
        this.longitude = position.coords.longitude
      },
      error => {
        console.log(error.message)
      })
    }
    },

    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },

    convertTime(unixTime) {
      let dt = new Date(unixTime * 1000);
      let h = dt.getHours();
      let m = "0" + dt.getMinutes();
      let t = h + ":" + m.substr(-2);
      return t;
    },
  },
};
</script>

<style>
@import "./App.css";
</style>
