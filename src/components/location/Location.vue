<template>
  <div class="page-wrapper">
    
    <h2 class="page-title"> Confirmar Localização</h2>

    <div class="wrapper">
      
      <div class="weather-centered">
        <div class="weather-image-wrapper">
          <img src="../../assets/images/location.svg" alt="Tempo Atual" class="location-image">
        </div>
      </div>

      <h3 class="address">{{ address }}</h3>

    </div>
    

    <router-link :to="{ name: 'Situation' }">
      <vulma-button class="button is-primary is-medium confirm" color='primary'>Confirmar</vulma-button> 
    </router-link>

  </div>
</template>
<script>
import { getLocation } from '../../services/location/LocationService'
import { getCityName } from '../../services/location/CityService'
import VueLocalStorage from 'vue-ls'
import Vue from 'vue'

Vue.use(VueLocalStorage)

export default {
  name: 'location',

  data () {
    return {
      address: '-'
    }
  },

  methods: {
    location () {
      getLocation()
        .then(this.saveInformation)
        .then(getCityName)
        .then(data => {
          this.address = data.endereco
        })
    },
    saveInformation (position) {
      Vue.ls.set('lat', position.lat)
      Vue.ls.set('lng', position.lng)
      return position
    }
  },

  mounted () {
    this.location()
  }
}
</script>
<style lang="stylus" scoped>
.location-image 
  width: 100px

.location-title
  font-size 28px
  color #fff
  margin: 50px 0;

.wrapper 
  background-color: #242833;
  padding: 60px;

.city 
  width: 100%;
  background: transparent;
  font-size: 25px;
  text-align: center;
  color: #fff
  height: 44px;
  border-radius: 11px
  border: .5px solid #ffffff

.form
  padding-top 50px

.page-wrapper
  font-family: 'Dosis', sans-serif;

.confirm
  padding-left: 2.75em
  padding-right: 2.75em
  transition: background-color .3s ease
  background-color #6a94e6
  width: 70%;
  max-width 300px
  border-radius: 100px;
  position fixed
  bottom 15px
  left 50%
  transform translate3d(-50%, 0, 0)
  font-size 1.25rem
  font-family: 'Dosis', sans-serif;

.confirm:active, .confirm:hover
  background-color #6086d0

.page-title {
    margin: 20px 0;
    font-size: 28px;
    font-weight: inherit;
    font-style: normal;
    color #fff
}

.address 
  padding-top 25px
  color #fff
  font-size 20px
  line-height 30px

.edit
  background: #fada46
  margin-top: 20px
  color: #3a465a
  font-weight: 500
</style>
