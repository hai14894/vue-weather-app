<template>
  <div
    id="app"
    :class="
      typeof weather.main !== 'undefined' && weather.weather[0].main === 'Clear'
        ? 'clear'
        : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search by city name..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          
          <div class="date">{{ getDate() }}</div>
        </div>
        
        <div class="weather-box">
          <div class="weather">{{ weather.weather[0].description }}</div> 
          <div class="temp">
            {{ Math.round(weather.main.temp) }}°c<img
              width="100px"
              :src="iconSrc"
            />
          </div>
          <div class="weather"></div>
          <div class="more-info">
            <div class="high">High: {{ Math.round(weather.main.temp_max) }}°c</div>
            <div class="low">Low: {{ Math.round(weather.main.temp_min) }}°c</div>
            <div class="wind">Wind speed: {{ weather.wind.speed }} km/h</div>
            <div class="pressure">Pressure: {{ weather.main.pressure }} hPa</div>
            <div class="humidity">Humidity: {{ weather.main.humidity }}%</div>
            <div class="visibility">Visibility: {{ weather.visibility/1000}} km</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      api_key: "6fbbe641ce030c39837ac25233abca8c",
      url: "http://api.openweathermap.org/data/2.5/weather?q=",
      query: "",
      weather: {},
      iconSrc: "",
    };
  },
  mounted() {
    axios
      .get(`${this.url}Sydney&appid=${this.api_key}&units=metric`)
      .then((res) => {
        this.weather = res.data;
        this.iconSrc = `http://openweathermap.org/img/wn/${this.weather.weather[0].icon}@2x.png`;
      })
      .catch((error) => console.log(error));
  },

  methods: {
    async fetchWeather(e) {
      if (e.key == "Enter") {
        await axios
          .get(`${this.url}${this.query}&appid=${this.api_key}&units=metric`)

          .then((res) => {
            this.weather = res.data;
            this.iconSrc = `http://openweathermap.org/img/wn/${this.weather.weather[0].icon}@2x.png`;
            this.query = "";
          })

          .catch((err) => console.log(err));
      }
    },
    getDate() {
      return new Date().toLocaleDateString();
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=David+Libre:wght@700&display=swap');
.more-info{
  text-align: center;
  margin: 15px;
  color: rgb(56, 52, 45);
  font-size: 25px;
  font-style: italic;
  display: inline-block;
  background-color: rgba(255, 255, 255, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  padding:10px;
  border-radius:16px
  
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'David Libre', serif;
}

#app {
  background-image: url("./assets/cloudy.jpeg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  
}

#app.clear {
  background-image: url("./assets/clear.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.15),
    rgba(0, 0, 0, 0.15)
  );
}
.search-box {
  display: table;
  margin: 0 auto;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  margin-bottom: 10px;

  color: #313131;
  font-size: 15px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);

}
.location-box .location {
  color: rgba(155, 243, 239, 0.89);
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
  font-size: 75px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 15px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 35px;
  font-weight: 700;
  font-style: italic;

}
</style>
