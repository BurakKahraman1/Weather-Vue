<template>
  <div>

    <div class="header-top">
      <div class="titleContainer">
        <nuxt-link class="title" to="/">
          WEATHER APP
        </nuxt-link>
      </div>
      <div class="imgContainer">
        <img :src="require(`../assets/images.svg`)" class="logo" />
      </div>

      <form @submit.prevent="handleSearch" class="form">
          <input type="text" id="searchbar2" placeholder="City" v-model="searchedCity" />
          <input type="submit" class="search" value="Search" />
      
      </form>

    </div>
   <div v-for="pickedCity in pickedCity">
    <div class="containerHead">
      <div v-if="apiCalled" class="mainTheme">
       <div class="detailHeader">
        <div class="grid">
          <img class="image" :src="`http://openweathermap.org/img/wn/${pickedCity.weather[0].icon}@2x.png`" />
        </div>
        <div class="grid">
         <p class="titles one">{{ pickedCity.name }}</p>
         <p class="descriptions only">{{pickedCity.weather[0].description.toUpperCase()}}</p>
       </div>
       <div class="grid">
         <p class="titles cntry">{{pickedCity.sys.country}}</p>
        </div>
        </div>
           
        <div class="contain">
          <div  class="grid">
            <p class="descriptions">TEMPERATURE: {{ Math.round(pickedCity.main.temp / 11) }} C</p>
            <p class='descriptions'>WIND: {{ pickedCity.wind.speed }} Km/h</p>
          </div>
          <div  class="grid">
            <p class='descriptions'>PRESSURE: {{ pickedCity.main.pressure }} p</p>

            <p class='descriptions'>HUMIDITY: {{ pickedCity.main.humidity }} Km/h</p>
          </div>
          <div  class="grid">
            <p class='descriptions'>WIND DEGREE: {{ pickedCity.wind.deg }} C</p>
            <p class='descriptions'>COORDINATION: {{ Math.round(pickedCity.coord.lat) }},
              {{ Math.round(pickedCity.coord.lon) }}</p>

          </div>
        </div>

      </div>
      <div v-else class="mainTheme">
        <p class="title">Loading....</p>
      </div>
    </div>
  </div> 

  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'AppHeader',
  data() {
    return {
      searchedCity: null,
      pickedCity: [],
      apiCalled: false,
    
    }
  },
  methods: {
    handleSearch() {
      if(this.pickedCity.length>=3){

        this.pickedCity.splice(3, 1)
      }

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.searchedCity}&appid=17a886603a7874da0530b1008b8d9e99`)
      .then(res => this.pickedCity.unshift(res.data))
      .catch(err => console.log(err))
      .finally(()=>this.apiCalled=true)
      this.searchedCity=null
    }
  },
  mounted() {
    
    // if(navigator.geolocation){
      //   navigator.geolocation.getCurrentPosition(position=>console.log(position))
      // }
      
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=istanbul&appid=17a886603a7874da0530b1008b8d9e99`)
      .then(res => this.pickedCity.unshift(res.data))
      .catch(err => console.log(err))
      .finally(()=>this.apiCalled=true)
    }
  }

</script>

<style lang="scss">
@import '../assets/scss/header.scss';
@import '../assets/scss/detail.scss';
</style>

