<script setup>
import { ref, onMounted } from 'vue';
import { API_KEY, BASE_URL } from './constants';

const city = ref('Kyiv');
const weatherInfo = ref(null);

function getWeather() {
  fetch(`${BASE_URL}?q=${city.value}&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => (weatherInfo.value = data));
}

onMounted(getWeather);
</script>

<template>
  <div class="page">
    <div class="weather">
      <div class="city-inner">
        <input
          v-model="city"
          class="search"
          type="text"
          @keyup.enter="getWeather"
        />
      </div>
      <h2 class="city">Weather in: {{ city }}</h2>
      <h1 class="temperature">&deg;C</h1>
      <div class="flex">
        <img src="" class="icon" alt="Weather icon" />
        <div class="description"></div>
      </div>
      <div class="pressure">Pressure:</div>
      <div class="humidity">Humidity:</div>
      <div class="speed">Wind speed:</div>
      <div class="deg">Wind direction:</div>
    </div>
  </div>
</template>
