<script setup>
import { ref } from 'vue';
const appKey = "f98bfb710849d537c04c71302acdedcd";
const query = ref("");
const urlBase = "https://api.openweathermap.org/data/2.5/";
const weather = ref(null);

const fetchData = async () => {
  if (query.value) {
    try {
      const response = await fetch(
        `${urlBase}weather?q=${query.value}&units=metric&appid=${appKey}`
      );

      if (!response.ok) {
        throw new Error("Failed");
      }

      const data = await response.json();
      weather.value = data;
    } catch (error) {
      console.log(error);
    } finally {
      query.value = "";
    }
  }
};


</script>

<template>
     <center>    
  <div class="body">
<div class="container">
  <h1>Weather App</h1>
  <img src="../public/images/icons8-weather.svg" alt="">
</div>
<div class="search">
  <input type="text" v-model="query" @keyup.enter="fetchData" placeholder="Enter City Name">
  <button @click="fetchData">Check</button>
</div><br><br>
<div v-if="weather">
  <h2>{{ weather.name }},
    <span> {{ weather.sys.country}}</span>
  </h2>
  <div>
    <h3>{{Math.floor(weather.main.temp) }}
      {{ weather.weather[0].main.temp }} C
     ( {{ weather.weather[0].description }} )
    </h3>
  </div>
</div>
</div>
</center>
</template>
<style>
center{
  height: 800px;
  padding-top: 150px;
  background-color: rgba(27, 117, 236, 0.684);
}

img{
  width: 50px;
  height: 50px;
}
button{
  width: 50px;
  height: 20px;
  border: none;
  border-radius: 3px;
}
</style>