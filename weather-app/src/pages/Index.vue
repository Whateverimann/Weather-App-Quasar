<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input v-model="text" placeholder="Search" dark borderless>
        <template v-slot:before>
          <q-icon name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">Gdańsk</div>

        <div class="text-h6 text-weight-light">Rain</div>

        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>20</span>
          <span class="text-h4 degree relative-position">&deg;</span>
        </div>
      </div>

      <div class="col text-center">
        <img src alt />
      </div>
    </template>

    <template v-else>
      <div class="col text-center text-white">
        <div class="col text-h2 text-weight-thin">
          Weather
          <br />App
        </div>
        <q-btn class="col">
          <q-icon left size="3em" name="map" />
          <div>Label</div>
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
      weatherData: null
    };
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