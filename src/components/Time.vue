<template>
  <div class="time-container">
      <div id="time">{{ time }}</div>
      <div id="date">{{ date }}</div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import moment from 'moment';
@Component({
  data () {
    return {
      time: '',
      date: ''
    }
  }
})
export default class Time extends Vue {
   mounted() {
    this.tick()
  }

  tick() {
    const now = moment()
    this.$data.time = now.format('h:mm:ss A')
    this.$data.date = now.format('MMMM Do')
    var timer = window.setTimeout(this.tick, 1000);
  }
}
</script>
<style>
#time{
  font-size: 48pt;
}
#date{
  font-size: 14pt;
}
</style>
