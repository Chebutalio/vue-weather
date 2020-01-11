<template>
  <div class="app">
    <h1>{{ msg }}</h1>
    <section v-if="errored">
      <p class="error-text">Wrong city name, or service is not available at this time.</p>
    </section>
    <input class="city" type="text" v-model="city" placeholder="Enter the city name">
    <select class="country" v-model="country" name="country" id="country">
      <option disabled value="">Pick the country</option>
      <option value="ua">ua</option>
      <option value="ru">ru</option>
      <option value="uk">uk</option>
    </select>
    <button class="btn" @click="getInfo">Show Weather</button>
    <div v-if="info">
      <div class="weather-info">
        <ul class="cards">
          <li class="card-info" v-for="(info, index) in info" :key="index">
            <p class="date-title"> {{ getDate(info) }} </p>
            <p class="time-title">{{ getTime(info) }}</p>
            <div>
              <p>Min.Temp.: {{ info.main.temp_min }}</p>
              <p>Max.Temp.: {{ info.main.temp_max }}</p>
              <p>Feels like: {{ info.main.feels_like }}</p>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Weather',
  data: function () {
    return {
      title: 'Weather',
      city: '',
      country: '',
      info: null,
      loading: true,
      errored: false
    }
  },
  methods: {
    getInfo () {
      axios
        .get(`https://api.openweathermap.org/data/2.5/forecast?q=${this.city},${this.country}&units=metric&mode=json&APPID=8d892cf4558e0259c7af92b77e03b096`)
        .then(response => (this.info = response.data.list))
        .catch(error => {
          console.error(error)
          this.errored = true
        })
        .finally(() => (this.loading = false))
    },
    getDate: function (info) {
      return new Date(info.dt_txt).toLocaleDateString('uk-UA')
    },
    getTime: function (info) {
      return new Date(info.dt_txt).toLocaleTimeString('uk-UA')
    }
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 10px 10px;
}

.app {
  max-width: 1000px;
  margin: auto;
}

.city {
  width: 8rem;
  padding: .3rem 1rem;
  margin: 2rem;
  border-radius: 15px;
  background: #F2F2F2;
}

.city:focus {
  border-color: #E77D1B;
  outline: none;
  background: #FFF;
}

.country {
  width: 10rem;
  height: 1.8rem;
  padding: .3rem 1rem;
  margin: 1rem;
  cursor: pointer;
  border-radius: 15px;
}

.country:focus {
  border-color: #E77D1B;
  outline: none;
}

.country:hover {
  border-color: #E77D1B;
  background: #FFF;
}

.btn {
  width: 10rem;
  height: 1.8rem;
  padding: .3rem 1rem;
  margin: 1rem;
  cursor: pointer;
  border-radius: 15px;
}

.btn:focus {
  border-color: #E77D1B;
  outline: none;
}

.btn:hover {
  border-color: #E77D1B;
  background: #FFF;
}

.weather-info {
  border: 3px solid #E77D1B;
  border-radius: 15px;
}

.card-info {
  border: 1px dotted #AB1431;
  border-radius: 15px;
  padding: 0 1rem;
}

.date-title {
  color: #513CD1;
  font-size: 1.3rem;
}

.time-title {
  color: #2A5014;
  font-size: .8rem;
}

.error-text {
  color: #EF3D09;
  font-size: .8rem;
}
</style>
