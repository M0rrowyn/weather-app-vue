<script setup></script>

<template>
  <div class="page">
    <div class="weather">
      <div class="city-inner">
        <input type="text" class="search" />
      </div>
      <h2 class="city">Weather in:</h2>
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
