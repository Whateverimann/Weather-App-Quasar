<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input v-model="text" placeholder="Search" dark borderless>
        <template v-slot:before>
          <q-icon @click="getLocation" name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">{{weatherData.name}}</div>

        <div class="text-h6 text-weight-light">{{weatherData.weather[0].description}}</div>

        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{Math.round(weatherData.main.temp)}}</span>
          <span class="text-h4 degree relative-position">&deg;</span>
        </div>
      </div>

      <div class="col text-center">
        <img :src="`http://openweathermap.org/img/wn/01d@2x.png`" />
      </div>
    </template>

    <template v-else>
      <div class="col text-center text-white">
        <div class="col text-h2 text-weight-thin">
          Weather
          <br />App
        </div>
        <q-btn class="col" flat @click="getLocation">
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
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition(position => {
        console.log("position: ", position);
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;
        this.getWeatherByCoords();
      });
    },
    getWeatherByCoords() {
      this.$axios(
        `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}9&appid=${this.apiKey}&units=metric`
      ).then(response => {
        this.weatherData = response.data;
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
