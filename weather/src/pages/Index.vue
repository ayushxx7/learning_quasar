<template>
  <q-page class="flex column" :class="bgClass">
    <!-- Seach Bar -->
    <div class='col q-pt-lg q-px-md'>
      <q-input 
        @keyup.enter="getWeatherBySearch"
        filled 
        v-model="search" 
        placeholder="Search" 
        dark
        borderless
        >
        <template v-slot:before>
          <q-icon 
            @click="getLocation"
            name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn 
            @click="getWeatherBySearch"
            round dense flat icon="search" />
        </template>
      </q-input> 
    </div>

    <!-- Weather Info text -->
   <template v-if="weatherData">
     <div class="col text-white text-center">
       <div class='text-h4 text-weight-light'>
         {{weatherData.name}}
       </div>
       <div class="text-h6 text-weight-light">
         {{weatherData.weather[0].main}}
       </div>
       <div class="text-h1 text-weight-thin q-my-lg relative-position">
         <span>{{Math.round(weatherData.main.temp)}}</span>
         <span class='text-h4 relative-position degree'>&deg;C</span>
       </div>
     </div>
  
     <!-- Image div -->
     <div class="col text-center">
       <img :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`" alt='Bill'>
     </div>
  </template>
  <template v-else>
    <div class="col column text-center text-white">
      <div class="col text-h2 text-weight-thin">
        Quasar<br>Weather
      </div>
      <q-btn 
        @click="getLocation"
        class='col' flat>
        <q-icon left size="3em" name="my_location" />
        <div>Find my location</div>
      </q-btn>
    </div>
  </template>

   <!-- skyline -->
   <div class="col skyline">
   </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  computed: {
    bgClass() {
      if (this.weatherData) {
        if (this.weatherData.weather[0].icon.endsWith('n')) {
          return 'bg-night'
        }
        else {
          return 'bg-day'
        }
      }
    }
  },
  methods: {
    getLocation() {
      this.$q.loading.show()
      navigator.geolocation.getCurrentPosition(
        position => {
          console.log('Position:', position)
          this.lat = position.coords.latitude
          this.lon = position.coords.longitude
          this.getWeatherByCoords()
        }
      )
    },
    getWeatherByCoords() {
      this.$q.loading.show()
      this.$axios(`${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`).
        then(response => {
          this.weatherData = response.data
          this.$q.loading.hide()
        })
    },
    getWeatherBySearch() {
      this.$q.loading.show()
      this.$axios(`${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`).
        then(response => {
          this.weatherData = response.data
          this.$q.loading.hide()
        })
    }
  },
  data() {
    return {
      search: '',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      apiKey: '5a7452e418047f7f8ecf2ba5cd7b1c3a'
    }
  }

}
</script>

<style lang='sass'>
  .q-page
    background: linear-gradient(to bottom, #136a8a, #267871)
    &.bg-day
      background: linear-gradient(to top, #00d2ff, #3a7bd5)
    &.bg-night
      background: linear-gradient(to right, #232526, #414345)
  .degree
    top: -44px;
  .skyline
    flex: 0 0 100px
    background: url(../assets/town.png)
    background-size: contain
    background-position: center bottom
</style>
