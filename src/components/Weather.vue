<template>
  <div class="weather-container">
      <div id="overview">
        <div id="location">{{location}}</div>
        <img id="icon" v-bind:src="getIcon(visual.icon)"/>
        <span id="main-temp">{{temperature.overall}}&deg;F</span>
      </div>
      <div id="details">
          <div id="feels">(Feels like: {{temperature.feelsLike}}&deg;F)</div>
          <div id="range">
              H: {{temperature.minimum}}&deg;F
              <br/>
              L: {{temperature.maximum}}&deg;F
         </div>

      </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import axios from 'axios';
@Component({
    props: {
        zip: String,
        country: String
    },
   data () {
    return {
      location: String,
      temperature: Temperature,
      visual: Visual
    }
  }
})
export default class Weather extends Vue {
  
    // default so not ugly on load
    created(){
        this.$data.location = ''
    }
    mounted(){
        // need to tick every hour or so TODO
        var tick = () => {
            var weatherData = this.getCurrentWeather(process.env.VUE_APP_WEATHER_API, this.$props.zip, this.$props.country)
            this.assignProperties(weatherData)
            var timer = window.setTimeout(tick, 1800000);
        }
        tick()

    }

    getIcon(icon:String){
        return require('../assets/icons/'+ icon +'.png')
    }


    getCurrentWeather(apiKey:String, zip:String, country: String):Promise<WeatherData>{
        const requestUrl = 'http://api.openweathermap.org/data/2.5/weather?zip='
         + zip + ',' + country + '&appid=' + apiKey + '&units=imperial';
        let that = this;
        return axios.get(requestUrl).then(function (response) {   
               var location = response.data.name;
               var main = response.data.main;
               var visual = response.data.weather[0];
               return new WeatherData(
                        location,
                        new Temperature.builder()
                            .overall(Math.round(main.temp))
                            .range(Math.round(main.temp_min),Math.round(main.temp_max))
                            .feelsLike(Math.round(main.feels_like))
                            .build(),
                        new Visual(visual.main,visual.description,visual.icon))
        })
        .catch(function (error) {
                console.log(error)
                return Promise.reject(error)
        })
     }

    async assignProperties(promise:Promise<WeatherData>){
        var weatherData:WeatherData = await promise;
        this.$data.location = weatherData.location;
        this.$data.visual = weatherData.visual;
        this.$data.temperature = weatherData.temperature;
    }
}

    class WeatherData{
        location: String
        temperature: Temperature;
        visual: Visual
        constructor(location:String,temperature:Temperature,visual:Visual){
            this.location = location
            this.temperature = temperature
            this.visual = visual
        }
    }

    class Temperature{
        overall: number;
        minimum: number;
        maximum: number;
        feelsLike: number;
        private constructor(overall:number,min:number,max:number,feels:number){
            this.overall = overall
            this.minimum = min
            this.maximum = max;
            this.feelsLike = feels;
        }
        static builder = class {
            private o:any
            private min :any
            private max :any
            private feels :any

           overall(o:number){
            this.o = o;
            return this;
           }

           range(min:number,max:number){
            this.min=min;
            this.max=max;
            return this;
           }

           feelsLike(feels:number){
            this.feels=feels;
            return this;
           }

           build(): Temperature{
               if(this.o==null || this.min==null || this.max == null || this.feels ==null){
                   throw new Error("All fields are not set:" + this)
               }
              return new Temperature(this.o,this.min,this.max,this.feels);
           }
        }
    }
    class Visual{
        main: String;
        description: String;
        icon: String;
        constructor(main:String,description:String,icon:String){
            this.main = main;
            this.description = description;
            this.icon= icon ;
        }
    }
</script>
<style>
    #overview{
        text-align:right;
    }
    #location{
        padding-top:10px;
        font-size: 14pt;
    }
    #main-temp{
        font-size: 48pt;
    }
    #icon{
        height: 48pt;
    }
    #details{
         text-align:right;
    }
    #feels{
        padding-bottom:10px;
        font-size:10pt;
    }
    #range{
        font-size:18pt;
    }
</style>
