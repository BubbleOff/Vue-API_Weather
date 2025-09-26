<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-show="city !== ''" v-on:click="getWeather()">Получить погоду</button>
    <button disabled v-if="city === ''">Введите название города</button>
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
      return this.city == "" ? "ваше городе" : "«" + this.city + "»"
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Минимальная температура: " + this.info.main.temp_max
    },
   },
   methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа"
        return false
      }
      this.error = "";

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&lang=ru&appid=83f53423b1aa11617b3beeefb556a30e`)
        .then(res => (this.info = res.data)) // Получение и занесение ответа в info
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
