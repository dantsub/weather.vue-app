<template>
  <div id="app" :class="{warm: isWarm, cold: isCold}">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keyup.enter="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="weather.main !== undefined">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: 'b217250e76e01935a0129a9887df1b6f',
      base_url: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      isCold: true,
      isWarm: false,
    }
  },
  methods: {
    fetchWeather() {
      fetch(`${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
        .then((res) => res.json())
        .then(this.setResults)
    },
    setResults(res) {
      this.weather = res;
      this.toggleTemp();
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    toggleTemp() {
      const temp = (this.weather.main !== undefined && this.weather.main.temp > 16);
      this.isCold = !temp;
      this.isWarm = temp;
    }
  }
}
</script>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Montserrat', sans-serif;
}
#app {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.cold {
  background-image: url('./assets/cold-bg.jpg');
}
.warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
  rgba(0, 0, 0, 0.25),
  rgba(0, 0, 0, 0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  /* Resets */
  appearance: none;
  border: none;
  outline: none;
  background: none;
  /* Color and shape */
  box-shadow: 0 0 .5rem rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, .5);
  border-radius: 0 1rem;
  transition: 0.4s;
}
.search-bar:focus {
  background-color: rgba(255, 255, 255, .75);
  box-shadow: 0 0 1rem rgba(0, 0, 0, 0.25);
  border-radius: 1rem 0;
}
.location-box,
.weather-box {
  text-align: center;
}
.location {
  color: #FFF;
  font-size: 2rem;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, .25);
}
.date {
  color: #FFF;
  font-size: 1.25rem;
  font-weight: 300;
  font-style: italic;
}
.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 6.375rem;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, .25);
  border-radius: 1rem;
  margin-top: 1.875rem;
  margin-bottom: 1.875rem;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather {
  color: #FFF;
  font-size: 3rem;
  font-weight: 400;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
