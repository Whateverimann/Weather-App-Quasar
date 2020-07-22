<template>
  <q-page class="flex column" :class="bgClass">
    <div class="col q-pt-lg q-px-md">
      <q-input
        v-model="search"
        @keyup.enter="getWeatherBySearch"
        placeholder="Search"
        dark
        borderless
      >
        <template v-slot:before>
          <q-icon @click="getLocation" name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn @click="getWeatherBySearch" round dense flat icon="search" />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">{{weatherData.name}}</div>

        <div class="text-h6 text-weight-light">{{weatherData.weather[0].description}}</div>

        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{Math.round(weatherData.main.temp)}}</span>
          <span class="text-h4 degree relative-position">&deg;C</span>
        </div>
      </div>

      <div class="col text-center">
        <img :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`" />
      </div>
    </template>

    <template v-else>
      <div class="col text-center text-white">
        <div class="col text-h2 text-weight-thin">
          Weather
          <br />App
        </div>
        <q-btn class="col q-pt-lg" flat @click="getLocation">
          <q-icon left size="3em" name="my_location" />
          <div>Find my location</div>
        </q-btn>
      </div>
    </template>

    <div class="col forest"></div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: "https://api.openweathermap.org/data/2.5/weather",
      apiKey: "856777ea40e28721fef25105b39164db"
    };
  },
  computed: {
    bgClass() {
      if (this.weatherData) {
        if (this.weatherData.weather[0].icon.endsWith("n")) {
          return "bg-night";
        } else {
          return "bg-day";
        }
      }
      return "";
    }
  },
  methods: {
    getLocation() {
      this.$q.loading.show();
      navigator.geolocation.getCurrentPosition(position => {
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;
        this.getWeatherByCoords();
        this.$q.loading.hide();
      });
    },
    getWeatherByCoords() {
      this.$q.loading.show();
      this.$axios(
        `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`
      ).then(response => {
        this.weatherData = response.data;
        this.$q.loading.hide();
      });
    },
    getWeatherBySearch() {
      this.$q.loading.show();
      this.$axios(
        `${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`
      ).then(response => {
        this.weatherData = response.data;
        this.$q.loading.hide();
      });
    }
  }
};
</script>
<style lang="scss">
body {
  font-family: "Poppins", sans-serif;
}
.q-page {
  background: linear-gradient(to bottom, #007991, #78ffd6);
  &.bg-night {
    background: linear-gradient(to bottom, #000428, #004e92);
  }
  &.bg-day {
    background: linear-gradient(to bottom, #2980b9, #6dd5fa, #ffffff);
  }
}
.degree {
  top: -44px;
}
.forest {
  flex: 0 0 100px;
  background: url(../statics/forest.png);
  background-size: contain;
  background-position: center bottom;
}
</style>
