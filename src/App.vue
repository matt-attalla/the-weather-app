<template>
  <div id="app">
    <main>
      <div class="search-box">
        <!-- used keyup here, where accept any keypress event -->
        <!-- the @keypress doesn't work -->
          <input type="text" class="search-bar" placeholder="input location" v-model="inputLocation" v-on:keyup="findweath" >
      </div>
      <div class="weatherwrap" v-if="typeof weather.main != 'undefined'">
        <div class="locationbox">
          <div class="location">
            {{weather.name}},{{weather.sys.country}}
          </div>
        </div>
        <div class="weatherbox">
          <div class="temp">
            {{weather.main.temp}} &#8451;
          </div>
          <div class="weatherbar">
            {{weather.weather[0].main}}
          </div>
        </div>
      </div>
      <weather-tabs></weather-tabs>
    </main>
  </div>
</template>

<script>
// import indexPage from './components/indexPage.vue'
import weatherTabs from '@/components/weatherTabs.vue'
export default {
  name: 'App',
  data() {
     return {
       //openweathermap api key and url
       api_key: process.env.VUE_APP_CURRENT_WEATHER_API_KEY,
       url_base: 'https://api.openweathermap.org/data/2.5/',
       //location infor and weather infor
       inputLocation: '',
       weather:{}
     }
   },
   components:{
      weatherTabs
   },
   methods:{
     //event listener
     findweath(e){
       //if the Enter key is pressed then acces the api
       if(e.key == "Enter"){
         //fetch to return the weather data
         fetch(`${this.url_base}weather?q=${this.inputLocation}&units=metric&APPID=${this.api_key}`)
           .then(res => {
             return res.json();
           }).then(this.setResults);
       }
     },
     setResults (results){
       //assign data to weather
       this.weather = results;
     }
   }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
.inputtext{
  float: left;
}
main{
  height: 500px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
}
.search-box .search-bar:focus{
  background-color: rgba(128, 134, 255, 0.75);
}
.locationbox .location{
  font-size: 64px;
  padding-top: 5px;
  padding-left: 25px;
}
.weatherbox .temp{
  font-size: 32px;
  padding-top: 5px;
  padding-left: 25px;
}
.weatherbox .weatherbar{
  font-size: 32px;
  padding-top: 5px;
  margin-top: 5px;
  padding-left: 25px;
}
</style>
