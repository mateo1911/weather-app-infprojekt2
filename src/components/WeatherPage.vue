<template>
 <div id="main-app">
  <div id="flex">
    <div>
      <img src="../assets/images/logo.svg" alt="">
    </div>
    <div id="location-info">
      <h2 id="city">{{weather.name}}</h2>
      <h2 id="country"> {{weather.sys.country}}</h2>
      <button @click="$emit('changeLocation')">Promijeni Lokaciju</button>
    </div>
  </div>

<div  id="weather">
    <div class="temperature">
      <img :src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}@4x.png`" alt="">
      <h1>{{weather.main.temp}}<span>°C</span> </h1>
      <div style="text-transform=capitalize">{{weather.weather[0].main}}</div>
    </div>

  <section>
      <div class="weather-info">
        <img src="../assets/images/windy-wind-svgrepo-com.svg" alt="" class="weather-icons">
        <div>
          <h2 class="weather-value">Brzina Vjetra</h2>
          <h1>{{weather.wind.speed}}</h1>
          <h2 class="weather-unit">m/s</h2>
        </div>
  </div>
  <div class="weather-info">
        <img src="../assets/images/humidity.svg" alt="" class="weather-icons">
        <div>
          <h2 class="weather-value">Vlažnost</h2>
          <h1>{{weather.main.humidity}}</h1>
          <h2 class="weather-unit">%</h2>
        </div>
  </div>
  <div class="weather-info">
        <img src="../assets/images/cloud.svg" alt="" class="weather-icons">
        <div>
          <h2 class="weather-value">Oblačnost</h2>
          <h1>{{weather.clouds.all}}</h1>
          <h2 class="weather-unit">%</h2>
        </div>
  </div>
  <div class="weather-info">
        <img src="../assets/images/pressure.svg" alt="" class="weather-icons">
        <div>
          <h2 class="weather-value">Tlak Zraka</h2>
          <h1>{{weather.main.pressure}}</h1>
          <h2 class="weather-unit">hPa</h2>
        </div>
  </div>
  <div class="weather-info">
        <img src="../assets/images/direction.svg" alt="" class="weather-icons">
        <div>
          <h2 class="weather-value">Smjer Vjetra</h2>
          <h1>{{weather.wind.deg}}</h1>
          <h2 class="weather-unit">deg</h2>
        </div>
  </div>
  <div class="weather-info">
        <img src="../assets/images/visibility.svg" alt="" class="weather-icons">
        <div>
          <h2 class="weather-value">Vidljivost</h2>
          <h1>{{(weather.visibility/1000).toFixed(2)}}</h1>
          <h2 class="weather-unit">km</h2>
        </div>
  </div>
  </section>
</div>
</div>
</template>

<script>
export default {
  data () {
    return {
      weather: {},

      cityName: localStorage.getItem('cityName')
    }
  },
  methods: {
    async getWeatherData () {
      const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&units=metric&appid=cfae3e315896c9621cb243de080a62e3`)

      const data = await response.json()
      console.log(data)
      this.weather = data
      return this.weather
    },
    degToCompass (num) {
      const val = Math.floor((num / 45) + 0.5)
      const arr = ['N', 'NE', 'E', 'SE', 'S', 'SW', 'W', 'NW']
      return arr[(val % 8)]
    }
  },
  created () {
    this.getWeatherData()
  }
}
</script>

<style lang="scss">
</style>
