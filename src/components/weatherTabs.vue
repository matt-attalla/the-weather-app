<template>
    <div class="weathertabs">
        <button @click="selectTab(1)" class="button history">Weather History</button>
        <button @click="selectTab(2)" class="button forecast">Weather Forecast</button>

        <div v-if="currentTab == 1" class="historytab">
            <table>
                <tr>
                    <td>Day #1</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
               
                </tr>
                <tr>
                    <td>Day #2</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
            
                </tr>
                <tr>
                    <td>Day #3</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
                 
                </tr>
                <tr>
                    <td>Day #4</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
                   
                </tr>
                <tr>
                    <td>Day #5</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
                    
                </tr>
                <tr>
                    <td>Day #6</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
                    
                </tr><tr>
                    <td>Day #7</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
                 
                </tr>

            </table>
        </div>

        <div v-if="currentTab == 2" class="forecasttab">
            <table>
                <tr>
                    <td>Day #1</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
            
                </tr>
                <tr>
                    <td>Day #2</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
              
                </tr>
                <tr>
                    <td>Day #3</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
               
                </tr>
                <tr>
                    <td>Day #4</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
         
                </tr>
                <tr>
                    <td>Day #5</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
              
                </tr>
                <tr>
                    <td>Day #6</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
           
                </tr><tr>
                    <td>Day #7</td>
                    <td>C/F</td>
                    <td>Icon</td>
                    <td>Humidity</td>
                    <td>Pressure</td>
               
                </tr>

            </table>

        </div>
    </div>
</template>

<script>
export default {
    props: {
        value: String
    },
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
