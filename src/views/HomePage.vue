<template>
 <div id="home" v-if="home">
   <img src="../assets/images/logo.svg" alt="" id="logo">
  <div id="user-info">
    <div>
      <input type="text" id="city" v-model.lazy="cityName" placeholder="Unesite ime grada">
    </div>
  </div>

  <button id="getLocationInfo" @click="getCurrentLocation()">Pretraži</button>

  <div v-if="notFound" class="no-result">
    <img src="https://i.gifer.com/GviB.gif" alt="">
    <h1>Oops!  &nbsp; We are not able to get a result.</h1>
  </div>
 </div>
 <template v-if="weatherPage">
  <WeatherPage @changeLocation = changeLocation() />
 </template>
</template>

<script>
import WeatherPage from '@/components/WeatherPage.vue'

export default {
  name: 'HomePage',
  data () {
    return {
      notFound: false,
      home: true,
      weatherPage: false,

      cityName: '',
      alphaCode: '',

      openWeatherOptions: {
        method: 'GET',
        headers: {
          'X-RapidAPI-Key': 'cfae3e315896c9621cb243de080a62e3',
          'X-RapidAPI-Host': 'https://api.openweathermap.org/data/2.5/'

        }
      }
    }
  },

  components: {
    WeatherPage
  },
  $emits: ['changeLocation'],
  methods: {
    async getCurrentLocation () {
      try {
        if (this.cityName.length >= 3) {
          const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&appid=cfae3e315896c9621cb243de080a62e3`)
          const data = await response.json()
          console.log(data)
          localStorage.setItem('cityName', data.name)
          if (data.cod === 200) {
            this.home = false
            this.weatherPage = true
          } else {
            this.notFound = true
            this.home = true
            this.weatherPage = false
          }
          return data
        } else {
          this.notFound = true
          this.home = true
          this.weatherPage = false
        }
      } catch (error) {
        this.notFound = true
        this.home = true
        this.weatherPage = false
        console.log(error)
      }
    },
    changeLocation () {
      this.home = true
      this.weatherPage = false
      this.notFound = false
      this.cityName = ''
    }
  }
}
</script>

<style lang="scss">
</style>
