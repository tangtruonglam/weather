<template>
  <div class="main-wthree-row">
    <div class="agileinfo-text">
      <div class="date">{{weather.date}}</div>
      <h2>
        {{weather.temp}}
        <span>°C</span>
      </h2>
      <h4 style="text-align: center">
        <input
          v-model="weather.location"
          @keyup.enter="fetchWeather"
          class="transparent"
          placeholder="Enter a city"
        />
      </h4>
      <h6>{{weather.description}}</h6>
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'
import { MessageBox, Button } from 'element-ui'
import axios from 'axios'

@Component({
  components: {
    MessageBox,
  },
})
export default class Weather extends Vue {
  private options = {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
    hour12: false,
  }
  private prnDt = new Date().toLocaleTimeString('vi-vn', this.options)
  private weather: any = {
    location: 'Ha Noi, VN',
    description: '',
    wind: 0,
    cloud: 0,
    city: 'Ha Noi',
    country: 'VN',
    temp: 0,
    humidity: 0,
    pressure: 0,
    tempMin: 0,
    tempMax: 0,
    date: this.prnDt,
  }

  private created() {
    this.fetchWeather()
  }

  private getCurrentTime(): string {
    const today = new Date()
    const date =
      today.getFullYear() + '-' + (today.getMonth() + 1) + '-' + today.getDate()
    const time =
      today.getHours() + ':' + today.getMinutes() + ':' + today.getSeconds()
    return date + ' ' + time
  }

  private currentTime: string = this.getCurrentTime()

  private async fetchWeather() {
    let arrayAddress = this.weather.location.split(',')
    this.weather.city = arrayAddress[0]
    this.weather.country = arrayAddress[1]
    try {
      const api: string = `https://api.openweathermap.org/data/2.5/weather?q=${this.weather.city},${this.weather.country}&APPID=c8f33e60d8fb3f428dd92cb6e59ad647`
      const res = await axios.get(api)
      const weather = {
        cloud: res.data.clouds.all,
        humidity: res.data.main.humidity,
        pressure: res.data.main.pressure,
        temp: this.convertToC(res.data.main.temp),
        tempMin: this.convertToC(res.data.main.temp_min),
        tempMax: this.convertToC(res.data.main.temp_max),
        description: res.data.weather[0].description,
        wind: res.data.wind.speed,
        date: new Date().toLocaleTimeString('vi-vn', this.options),
        city: arrayAddress[0],
        country: arrayAddress[1],
        location: `${arrayAddress[0]}, ${arrayAddress[1]}`,
      }
      this.weather = weather
    } catch (error) {
      MessageBox.alert(
        'Không thể tìm thấy thời tiết ở thành phố hiện tại',
        'Oops',
        {
          confirmButtonText: 'OK',
        }
      )
    }
  }

  private convertToC(kelvin: number) {
    let celsius = kelvin - 273
    return Math.round(celsius)
  }
}
</script>


<style lang="scss" scoped>
.box-card {
  width: 100%;
  margin: 0 auto;
  margin-bottom: 20px;
}
.transparent {
  background-color: transparent;
  border: none;
  outline: none;
  color: white;
  font-size: 1em;
  width: 200px;
}
</style>
