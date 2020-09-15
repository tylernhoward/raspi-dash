<template>
  <div id="app">
    <div class="bg"/>
    <div class="overlay"/>
    <div id = "container">
      <Time/>
      <Weather zip="21230" country="us"/>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Time from './components/Time.vue';
import Weather from './components/Weather.vue';

@Component({
  components: {
    Time,
    Weather,
  },
})
export default class App extends Vue {
  
}
</script>

<style>
#container{
  z-index:9999;
  padding: 30px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: bold;
  font-size: 18pt;
  letter-spacing:2px;
  color: white;
}
.overlay{
  z-index: -99;
  opacity:1;
  background: linear-gradient(to bottom, black, transparent);
  position:absolute;
  width:100%;
  height:100%;
}
.bg{
  z-index:-999;
  position:absolute;
  background: url("https://images.squarespace-cdn.com/content/v1/586285593e00be7ce7d21acf/1581078407292-NSOSNBIYM82K2ZQJOR2R/ke17ZwdGBToddI8pDm48kLPswmMOqQZ9-Q6KHLjvbpZ7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UTcpTqfU-ZEsztPyQLxhSSK-PhJjRDDFQG0l3_ZnmWi1QjT9byXZM3ISxo3y1NRptg/BaltDrone2019-50.JPG?format=1000w");
  height:100%;
  width:100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
html,body {
  padding:0;
  margin:0;
  height: 100%;
  width:100%;
}
</style>
