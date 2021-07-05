<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && condition === 'Haze'
        ? 'haze'
        : '' || condition === 'Clouds'
        ? 'cloudy'
        : '' || condition === 'Rain'
        ? 'rain'
        : '' || condition === 'Clear'
        ? 'clear'
        : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          name=""
          id=""
          class="search-bar"
          placeholder="Enter location..."
          v-model="query"
          @keyup.enter="fetchWeather"
        />
        <img class="location-search" src="./assets/search-solid.svg" alt="" />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">
            {{ weather.weather[0].main }}
            <img :src="icon" alt="" />
          </div>
          <div class="desc">
            {{ weather.weather[0].description }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
require("dotenv").config();
export default {
  name: "App",
  data() {
    return {
      api_key: process.env.VUE_APP_APIKEY,
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      condition: "",
      icon: "",
    };
  },
  methods: {
    fetchWeather(e) {
     
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      
    },
    setResults(results) {
      this.weather = results;
      this.condition = this.weather.weather[0].main;
      this.icon = `http://openweathermap.org/img/w/${this.weather.weather[0].icon}.png`;
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
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Poppins";
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: center;
  transition: 0.5s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}
#app.clear {
  background-image: url("./assets/clear.jpg");
}
#app.cloudy {
  background-image: url("./assets/cloudy.jpg");
}
#app.rain {
  background-image: url("./assets/rain.jpg");
}
#app.haze {
  background-image: url("./assets/haze.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
  display: flex;
  align-items: center;
  justify-content: space-around;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
  border-radius: 10px;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}

.location-search {
  height: 2rem;
  margin-left: 10px;
  opacity: 0.5;
}

.location-box .location {
  margin-top: 5rem;
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.desc {
  font-size: 1.2rem;
  color: #fff;
}
</style>
