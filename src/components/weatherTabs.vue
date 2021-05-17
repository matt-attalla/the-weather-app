<template>
    <div class="weathertabs">
        <button @click="selectTab(1)" class="button history">Weather History</button>
        <button @click="selectTab(2)" class="button forecast">Weather Forecast</button>

        <div v-if="currentTab == 1" class="historytab">
            <table>
                <tr>
                    <th>Day</th>
                    <th>&#8451;/&#8457;</th>
                    <th></th>
                    <th>Humidity</th>
                    <th>Wind (mph/kmh)</th>
                    <th>Min/Max (&#8451;/&#8457;)</th>
                </tr>
                <tr v-for="weather in history_weather.forecast.forecastday" :key="weather.id">
                    <td>{{new Date(weather.date).toDateString()}}</td>
                    <td>{{weather.day.avgtemp_c}}&#176; / {{weather.day.avgtemp_f}}&#176;</td>
                    <td><img v-bind:src=weather.day.condition.icon /></td>
                    <td>{{weather.day.avghumidity}}%</td>
                    <td>{{weather.day.maxwind_mph}} / {{weather.day.maxwind_kph}}</td>
                    <td>{{weather.day.mintemp_c}}&#176;/{{weather.day.mintemp_f}}&#176; | {{weather.day.maxtemp_c}}&#176;/{{weather.day.maxtemp_f}}&#176;</td>
                </tr>

            </table>

        </div>

        <div v-if="currentTab == 2" class="forecasttab">
            <table>
                <tr>
                    <th>Day</th>
                    <th>&#8451;/&#8457;</th>
                    <th></th>
                    <th>Humidity</th>
                    <th>Wind (mph/kmh)</th>
                    <th>Min/Max (&#8451;/&#8457;)</th>
                </tr>
                <tr v-for="weather in forecast_weather.forecast.forecastday" :key="weather.id">
                    <td>{{new Date(weather.date).toDateString()}}</td>
                    <td>{{weather.day.avgtemp_c}}&#176; / {{weather.day.avgtemp_f}}&#176;</td>
                    <td><img v-bind:src=weather.day.condition.icon /></td>
                    <td>{{weather.day.avghumidity}}%</td>
                    <td>{{weather.day.maxwind_mph}} / {{weather.day.maxwind_kph}}</td>
                    <td>{{weather.day.mintemp_c}}&#176;/{{weather.day.mintemp_f}}&#176; | {{weather.day.maxtemp_c}}&#176;/{{weather.day.maxtemp_f}}&#176;</td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    // this is where we retrieved the forecast and historical weather data
    props: [
        'forecast_weather', 'history_weather'
    ],
    data: function() {
        return {
            currentTab: 0
        }
    },
    methods:{
        selectTab(selectedTab){
            this.currentTab = selectedTab
        }
    }
}
</script>

<style>
    .button {
        font-size: 30px;
        width: 50%
    }

    table {
        border-collapse: collapse;
        width: 100%;
    }

    td, th {
        font-size: 32px;
        border: 1px solid #dddddd;
        text-align: center;
        padding: 4px;
    }
</style>
