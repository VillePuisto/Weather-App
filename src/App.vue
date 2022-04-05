<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm' : ''
    "
  >
    <main>
      <div class="heading">
        <h1 class="head">Weather App</h1>
        <h2>Harjoitustyö</h2>
        <h3>Ville Puisto</h3>
      </div>
      <div class="search">
        <input
          type="text"
          class="search-input"
          placeholder="Search..."
          v-model="query"
          v-on:keypress="fetchWeather"
        />
      </div>

      <div class="weather" v-if="typeof weather.main != 'undefined'">
        <div class="location-div">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateShow() }}</div>
        </div>

        <div class="weather-div">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather-status">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateShow () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Monday" ];

      let day = days.[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
.heading {
  text-align: center;
}
.heading .head {
  display: inline-block;
  padding: 15px 30px;
  color: white;
  font-size: 100px;
  font-weight: 850;

  text-shadow: 8px 10px rgba(0, 0, 0, 0.45);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 60px;
  margin: 35px 0px;

  box-shadow: 8px 10px rgba(0, 0, 0, 0.45);
}
* {
  margin: 5;
  padding: 5;
  box-sizing: border-box;
}
body {
  font-family: "Helvetica", sans-serif;
}
#app {
  background-image: url("./assets/mountain-1848342_1280.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.6s;
}

#app.warm {
  background-image: url("./assets/landscape-1844230_1280.png");
}

main {
  min-height: 100vh;
  padding: 30px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.65)
  );
}
.search-input {
  width: 100%;
  margin-bottom: 30px;
}
.search .search-input {
  display: block;
  width: 100%;
  padding: 15px;

  color: black;
  font-size: 25px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.45);
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 0px 20px 0px 20px;
  transition: 0.6s;
}

.search.search-input: focus {
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.85);
  border-radius: 16px 0px 16px 0px;
}

.location-div .location {
  color: white;
  font-size: 40px;
  font-weight: 600;
  text-align: center;
  text-shadow: 1px 4px rgba(0, 0, 0, 0.45);
}

.location-div .date {
  color: white;
  font-size: 20px;
  font-weight: 400;
  text-style: italic;
  text-align: center;
}

.weather {
  text-align: center;
}

.weather .temp {
  display: inline-block;
  padding: 15px 30px;
  color: white;
  font-size: 100px;
  font-weight: 850;

  text-shadow: 5px 8px rgba(0, 0, 0, 0.45);
  background-color: rgba(255, 255, 255, 0.45);
  border-radius: 20px;
  margin: 35px 0px;

  box-shadow: 5px 8px rgba(0, 0, 0, 0.45);
}

.weather .weather-status {
  color: white;
  font-size: 65px;
  font-weight: 750;
  font-style: italic;
  text-shadow: 5px 8px rgba(0, 0, 0, 0.45);
}
</style>
