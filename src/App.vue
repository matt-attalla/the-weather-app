<template>
  <div id="app">
    <main>
      <div class="search-box">
        <!-- used keyup here, where accept any keypress event -->
        <!-- the @keypress doesn't work -->
          <input type="text" class="search-bar" placeholder="input location" v-model="inputLocation" v-on:keyup="findweath" >
          <button v-on:click="toggleUnits" class="unit-toggle">&#8457;/&#8451;</button>
      </div>
      <div class="weatherwrap" v-if="Object.keys(current_weather).length !== 0">
        <div class="locationbox">
          <div class="location">
            {{current_weather.name}},{{current_weather.sys.country}}
          </div>
        </div>
        <div class="weatherbox">
        <div>
          <img v-bind:src="`http://openweathermap.org/img/wn/${current_weather.weather[0].icon}.png`" />
        </div>
          <div class="temp">
              {{Math.round(current_weather.main.temp)}} {{ weatherInFahrenheit ?  "&#8457;" : "&#8451;" }}
            <p> Max: {{Math.round(current_weather.main.temp_max)}} {{ weatherInFahrenheit ?  "&#8457;" : "&#8451;" }} </p> 
            <p> Min: {{Math.round(current_weather.main.temp_min)}} {{ weatherInFahrenheit ?  "&#8457;" : "&#8451;" }}</p> 
          </div>
        </div>
      </div>
      <div>
        <!-- this is where we pass the data to the forecast and historicall weather tabs -->
        <weather-tabs :forecast_weather=forecast_weather :history_weather=history_weather />
      </div>
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
       //openweathermap api key and url for current weather
       current_weather_api_key: process.env.VUE_APP_CURRENT_WEATHER_API_KEY,
       current_weather_url_base: 'https://api.openweathermap.org/data/2.5/',

       //theweatherapi api key and url for forecast and history weather
       forecast_and_history_weather_api_key: process.env.VUE_APP_FORECAST_HISTORY_WEATHER_API_KEY,
       forecast_and_history_weather_url_base: 'http://api.weatherapi.com/v1/',
       //location infor and weather infor
       inputLocation: '',
       current_weather:{},
       forecast_weather:{},
       history_weather:{},
       weatherInFahrenheit: true
     }
   },
   components:{
      weatherTabs
   },
   methods:{
     //event listener
     findweath(e){
       //if the Enter key is pressed then acces the apis
       if(e.key == "Enter"){
          //fetch to return the current weather data
          fetch(`${this.current_weather_url_base}weather?q=${this.inputLocation}&units=imperial&APPID=${this.current_weather_api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setCurrentWeatherResults);
        }
      },
           
      setCurrentWeatherResults (results){
        //assign data to weather
        this.current_weather = results;

        /* fetch the other data here since the weather api doesn't search with zip code
        the solution is to pass the zip code to the openweather api, and retrieve result, and pass the name of the city
        to the other api*/
        // fetch the forecast weather data
        fetch(`${this.forecast_and_history_weather_url_base}forecast.json?key=${this.forecast_and_history_weather_api_key}&q=${this.current_weather.name}&days=3&aqi=no&alerts=no`)
          .then(res => {
            this.weatherInFahrenheit = true
            return res.json();
          }).then(this.setForecastWeatherResults);

        
        // fetch the history weather data
        // get the date to start with
        var currentDate = new Date();
        var historyDateToStart = new Date();
        historyDateToStart.setDate(currentDate.getDate() - 6);
  
        fetch(`${this.forecast_and_history_weather_url_base}history.json?key=${this.forecast_and_history_weather_api_key}&q=${this.current_weather.name}&dt=${historyDateToStart.getFullYear()}-${historyDateToStart.getMonth() + 1}-${historyDateToStart.getDate()}&end_dt=${currentDate.getFullYear()}-${currentDate.getMonth() + 1}-${currentDate.getDate()}`)
          .then(res => {
            return res.json();
          }).then(this.setHistoryWeatherResults);
      },
      setForecastWeatherResults (results){
        //assign data to weather
        this.forecast_weather = results;
      },
      setHistoryWeatherResults (results){
        //assign data to weather
        this.history_weather = results;
        
      }, toggleToCelcius () {
          this.current_weather.main.temp = this.calcTempInCelcius(this.current_weather.main.temp)
          this.current_weather.main.temp_max = this.calcTempInCelcius(this.current_weather.main.temp_max)
          this.current_weather.main.temp_min = this.calcTempInCelcius(this.current_weather.main.temp_min)
          this.weatherInFahrenheit = false

      }, calcTempInCelcius (temp) {
          return (temp - 32) * (5/9)
        }, 

        calcTempInFahrenheit(temp) {
          return (temp * (9/5) ) + 32 
        }, 
        
        toggleToFarenheit () {
          this.current_weather.main.temp = this.calcTempInFahrenheit(this.current_weather.main.temp)
          this.current_weather.main.temp_max = this.calcTempInFahrenheit(this.current_weather.main.temp_max)
          this.current_weather.main.temp_min = this.calcTempInFahrenheit(this.current_weather.main.temp_min)
          this.weatherInFahrenheit = true

      }, toggleUnits () {
        if(this.weatherInFahrenheit) {
          this.toggleToCelcius()
        } else {
          this.toggleToFarenheit()
        }
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
.search-box .search-bar, .unit-toggle{
  padding: 15px;
  font-size: 20px;
}

.weatherwrap {
  padding-top: 20px; 
  padding-bottom: 20px; 
  background: lightblue;
  /* margin: 50px 0;  */
  text-align: center;
}
.search-box {
  display: flex;
  width: 100%;
}

.search-bar {
  flex-basis: 80%;
  width: 80%;
}

.unit-toggle {
  flex-basis: 20%;
  width: 20%;
}

.locationbox .location{
  background: lightblue;
  font-size: 64px;
}
.weatherbox .temp{
  background: lightblue;
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
