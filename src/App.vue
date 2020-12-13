<template>
  <div id="app" :class="generateClass">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
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
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>     
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      api_key: "67f60b434c1debca419e38ffe42c01e2",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
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
    dateBuilder() {
      let date = new Date();

      const options = {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric",
        hour: "numeric",
        minute: "numeric",
      };

      return new Intl.DateTimeFormat("en-GB", options).format(date);
    },
  },
  computed: {
    generateClass() {
      if (typeof this.weather.main == "undefined") {
        return "default";
      }
      return {
        clear: this.weather.weather[0].main.toLowerCase() === "clear",
        rain: this.weather.weather[0].main.toLowerCase() === "rain",
        clouds: this.weather.weather[0].main.toLowerCase() === "clouds",
        snow: this.weather.weather[0].main.toLowerCase() === "snow",
        fog: this.weather.weather[0].main.toLowerCase() === "fog",
        mist: this.weather.weather[0].main.toLowerCase() === "mist",
        default: true
      };
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
  background: #eeeeee;
  font-family: monospace, sans-serif;
}

#app {
  width: 380px;
  height: 600px;
  margin: 0 auto;
  margin-top: 30px;
  border-radius: 15px;
  box-shadow: 0px 0px 9px 5px #607D8B;
  border: 4px solid #607D8B;

  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.default {
  background-image: url("./assets/cold.jpg");
}
#app.clear {
  background-image: url("./assets/clear.jpg");
}
#app.clouds {
  background-image: url("./assets/clouds.jpg");
}
#app.fog {
  background-image: url("./assets/fog.jpg");
}
#app.mist {
  background-image: url("./assets/mist.jpg");
}
#app.rain {
  background-image: url("./assets/rain.jpg");
}
#app.snow {
  background-image: url("./assets/snow.jpg");
}

main {
  min-height: 100%;
  padding: 25px;
  border-radius: 12px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
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
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
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
  font-size: 82px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
