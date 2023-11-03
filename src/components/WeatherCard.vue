<script setup>
import { ref, onMounted, computed } from 'vue';
import { API_KEY, BASE_URL } from '@/constants';

const city = ref('');
const weatherInfo = ref(null);
const weatherError = ref('')

function getWeather() {
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => {
      if (data.cod === "400" || data.cod === "404") {
        weatherInfo.value = null
        weatherError.value = data.message
        return
      }
      
      weatherError.value = ''
      weatherInfo.value = data
    })

}

const weatherItemInfo = computed(() => {
  if (!weatherInfo.value || !weatherInfo.value.weather?.length) {
    return null;
  }

  return weatherInfo.value.weather[0];
});

const weatherIconUrl = computed(() => {
  if (!weatherItemInfo.value) {
    return null;
  }

  const openWeatherUrl = 'https://openweathermap.org/img/wn/';
  const weatherIconCode = weatherItemInfo.value.icon;

  return `${openWeatherUrl}${weatherIconCode}.png`;
});

const weatherDescription = computed(() => {
  if (!weatherItemInfo.value) {
    return null;
  }

  return weatherItemInfo.value.description;
});

onMounted(() => {
  if (city.value) {
    getWeather()
  }
});
</script>

<template>
  <div class="weather">
    <div class="city-inner">
      <input
        v-model="city"
        class="search"
        type="text"
        placeholder="Search by city name"
        @keyup.enter="getWeather"
      />
    </div>
    <div v-if="weatherError">{{ weatherError }}</div>
    <div class="wrapper" v-if="weatherInfo">
      <h2 class="city">Weather in: {{ city }}</h2>
      <h1 class="temperature">
        {{ Math.round(weatherInfo?.main?.temp) }}&deg;C
      </h1>
      <div v-if="weatherItemInfo" class="flex">
        <img
          v-if="weatherIconUrl"
          :src="weatherIconUrl"
          class="icon"
          alt="Weather icon"
        />
        <div v-if="weatherDescription" class="description">
          {{ weatherDescription }}
        </div>
      </div>
      <div class="pressure">
        Pressure: {{ weatherInfo?.main?.pressure }} hPa
      </div>
      <div class="humidity">Humidity: {{ weatherInfo?.main?.humidity }} %</div>
      <div class="speed">Wind speed: {{ weatherInfo?.wind?.speed }} km/h</div>
      <div class="deg">Wind direction: {{ weatherInfo?.wind?.deg }} &deg;</div>
    </div>
  </div>
</template>
