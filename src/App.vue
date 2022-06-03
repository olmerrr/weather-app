<template>
  <div id="app">
    <main class="main">
      <div class="container">
        <div class="search-box">
          <input
            type="text"
            class="search-bar"
            placeholder="Search City"
            autofocus
            v-model="query"
            @keypress="fetchWeather"
          />

          <!-- <button @click="fetchWeather">Search</button> -->
        </div>

        <div
          class="weather-wrap"
          v-if="typeof weather.condition !== 'undefined'"
        >
          <div class="location-box">
            <div class="location">
              {{ location.name }} {{ location.country }}
            </div>
            <div class="date">{{ location.localtime }}</div>
          </div>

          <div class="weather-box">
            <div class="temp">
              {{ weather.feelslike_c }}
            </div>

         
            <div class="weather">{{ weather.condition }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
  export default {
    name: 'App',
    components: {},
    data() {
      return {
        url_base: 'http://api.weatherapi.com/v1/current.json',
        api_key: 'ccff10f7cbfd4d1b815130739220306',
        query: '',
        weather: {},
        location: {},
      };
    },
    methods: {
      async fetchWeather(e) {
        if (e.key == 'Enter') {
          try {
            this.weather = {};
            const response = await fetch(
              `${this.url_base}?key=${this.api_key}&q=${this.query}`
            );
            const data = await response.json();
            this.setResults(data);
            return data;
          } catch (error) {
            console.error(error.message);
          }
        }
      },
      setResults(results) {
        this.weather = results.current;
        this.location = results.location;
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

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    background-image: url('./assets/cold.jpg');
    background-size: cover;
    background-position: bottom;
  }
  .main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.25),
      rgba(0, 0, 0, 0.75)
    );
  }
  .container {
    max-width: 1260px;
    margin: 0 auto;
  }
  .search-box {
    width: 100%;
    margin-bottom: 32px;
  }

  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #333;
    font-size: 18px;
    appearance: none;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
    border: none;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0 16px 0 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus {
    box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.72);
    border-radius: 16px 0 16px 0;
  }

  .weather-wrap {
  }
  .location {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .date {
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    text-align: center;
    font-style: italic;
  }
  .weather-box {
    text-align: center;
  }
  .weather-box .legend {
    font-size: 18px;
  }
  .temp {
    color: #fff;
    font-size: 102px;
    font-weight: 900;
    display: inline-block;
    padding: 10px 25px;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
  .weather {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
</style>
