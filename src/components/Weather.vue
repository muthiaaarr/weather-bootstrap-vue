<template>
  <div id="weather">
    <b-container class="bv-example-row">
      <b-row>
        <b-col cols="12" class="col">
          <b-form class="search-bar">
            <label class="sr-only" for="inline-form-input-name">Name</label>
            <b-input-group size="lg">
              <b-input-group-prepend is-text>
                <b-icon icon="search"></b-icon>
              </b-input-group-prepend>
              <b-form-input
                id="inline-form-input-name"
                class="form-input"
                placeholder="Search..."
                v-model="query"
                @keypress="runApi(e)"
              >
              </b-form-input>
            </b-input-group>
          </b-form>
        </b-col>
      </b-row>
      <div class="detail">
        <b-row class="text-center">
          <b-col>
            <h3 class="location">Bandung, Indonesia</h3>
          </b-col>
        </b-row>
        <b-row class="text-center">
          <b-col>
            <h3 class="time">{{dateFormat()}}</h3>
          </b-col>
        </b-row>
        <b-row class="text-center">
          <b-col>
            <b-button
              :pressed="true"
              variant="outline-info"
              size="lg"
              class="button-temp"
              ><span class="temp">9°C</span></b-button
            >
          </b-col>
        </b-row>
        <b-row class="text-center">
          <b-col>
            <h3 class="weather2">Rain</h3>
          </b-col>
        </b-row>
      </div>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "Weather",
  data() {
    return {
      api_key: "e12fa74525c1b00e0a3c27e1d46f156a",
      url_base: "http://api.openweathermap.org/data/2.5/",
      cors: 'https://cors-anywhere.herokuapp.com/',
      query: "",
      weather: {},
    };
  },
  methods: {
      runApi(e) {
        const url = `${this.cors}${this.url_base}weather?q=${this.query}&APPID=${this.api_key}`
        if (e.key === 'Enter') {              
            axios.get(url)
            .then(res => {
                console.log(res.data)
            })
        }
      },
      dateFormat() {
          let today = new Date()
          const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
          const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']

          let day = days[today.getDay()]
          let date = today.getDate()
          let month = months[today.getMonth()]
          let year = today.getFullYear()

          return `${day}, ${month} ${date}, ${year}`
      },
  },
};
</script>

<style scoped>
#weather {
  font-family: monospace;
  font-size: 30px;
}

.search-bar {
  margin-top: 20%;
  width: 100%;
}

.form-input {
  border-radius: 10px;
  font-size: xx-large;
}

h1 {
  font-weight: 500;
  color: azure;
  font-style: oblique;
  margin-top: 10%;
}

h6 {
  color: azure;
}

.detail {
  margin-top: 5%;
}

.location {
  color: azure;
  font-size: 50px;
  font-weight: bold;
}

.time {
  color: azure;
  font-size: 30px;
  font-style: italic;
}

.button-temp {
    border-radius: 10px;
    width: 30%;
    margin-top: 3%;
}

.temp {
  font-size: 50px;
  color: azure;
}

.weather2 {
  color: azure;
  font-size: 40px;
  font-style: italic;
  margin-top: 3%;
}
</style>