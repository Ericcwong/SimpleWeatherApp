<template>
  <div
    class="contain"
    :class="typeof weather.main != 'undefined' && weather.main.temp > 70 ? 'warm' : ''"
  >
    <main>
      <div class="searchBox">
        <input
          type="text"
          class="searchBar"
          placeholder="Location"
          v-model="query"
          v-on:keypress="fetchWeather"
        />
      </div>
      <div class="weatherWrap" v-if="typeof weather.main != 'undefined'">
        <div class="locationBox">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{getDate()}}</div>
        </div>
        <div class="weatherBox">
          <div class="temp">{{Math.round(weather.main.temp)}}°F</div>
          <div class="weather">{{weather.weather[0].description}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      api_key: 'c0d97a019ea859a14447316fcc3b3bce',
      urlBase: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == 'Enter') {
        fetch(
          `${this.urlBase}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json()
          })
          .then(this.setResults)
      }
    },
    setResults(results) {
      this.weather = results
      console.log(results)
    },
    getDate() {
      let d = new Date()
      let months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December',
      ]
      let days = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday',
      ]
      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()
      return `${day}, ${month}, ${date}, ${year}`
    },
  },
}
</script>

<style scoped>
* {
  /* margin: 0; */
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
}
.contain {
  background-image: url('/images/cold-bg.jpg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  transition: 0.4s;
}
.contain.warm {
  background-image: url('/images/warm-bg.jpg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
main {
  min-height: 100vh;

  padding: 25px;
  /* background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0, 75)
  ); */
}
.searchBox {
  width: 100%;
  margin-bottom: 30px;
}
.searchBox .searchBar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.searchBox .searchBar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.locationBox .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: rgba(0, 0, 0, 0.25);
}
.locationBox .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weatherBox {
  text-align: center;
}
.weatherBox .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weatherBox .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: rgba(0, 0, 0, 0.25);
}
</style>
