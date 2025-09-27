<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <p>Find out the weather in {{ cityName }}</p>
    <input type="text" v-model="city" placeholder="Enter city/state">
    <button v-show="city !== ''" v-on:click="getWeather()">Find out the weather</button>
    <button disabled v-if="city === ''">Enter the name of the city/state</button>
    <p class="error">{{ error }}</p>

    <div v-if="info !== null">
      <p>{{ showTemp }}°</p>
      <p>{{ showFeelsLike }}°</p>
      <p>{{ showMinTemp }}°</p>
      <p>{{ showMaxTemp }}°</p>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios';
export default {
   data() {
    return {
      city: "",
      error: "",
      info: null
    }
   },
   computed: {
    cityName() {
      return this.city == "" ? "your city/state" : "«" + this.city + "»"
    },
    showTemp() {
      return "Temperature: " + this.info.main.temp
    },
    showFeelsLike() {
      return "It feels like: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Minimum temperature: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Maximum temperature: " + this.info.main.temp_max
    },
   },
   methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "A name of more than one character is required."
        return false
      }
      this.error = "";

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&lang=ru&appid=API`)
        .then(res => (this.info = res.data)) // Receiving and entering a response in info
    }
   }
}
</script>

<style scoped>
.error {
  color: #d03939;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #FDBDBA;
  text-align: center;
  color: #DE5D83;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
  font-size: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 16px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper input::placeholder {
  color: #fcfcfc;
  font-style: italic;
}

.wrapper button:disabled {
  background: #EE9086;
  cursor: not-allowed;
}

.wrapper button {
  background: #E75480;
  color: #fff;
  font-size: 16px;
  border-radius: 10px;
  border: 2px solid #D71868;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
