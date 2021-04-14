<template>
  <div id="weather">
    <b-container class="bv-example-row">
      <b-row>
        <b-col cols="12" class="col">
          <b-form class="search-bar">
            <label class="sr-only" for="inline-form-input-name">Name</label>
            <b-input-group size="lg">
              <b-form-input
                id="inline-form-input-name"
                class="form-input"
                placeholder="Search..."
                v-model="query"
              >
              </b-form-input>
              <b-button variant="primary" size="lg" @click="runApi">
                <b-icon icon="search"></b-icon>
              </b-button>
            </b-input-group>
          </b-form>
        </b-col>
      </b-row>
      <div class="detail">
        <b-row class="text-center">
          <b-col>
            <h3 class="location" v-if="this.city !== ''">
              {{ this.city }}, {{ this.country }}
            </h3>
            <h3 class="location" v-else>Select your city/country</h3>
          </b-col>
        </b-row>
        <b-row class="text-center">
          <b-col>
            <h3 class="time">{{ dateFormat() }}</h3>
          </b-col>
        </b-row>
        <b-row class="text-center">
          <b-col>
            <b-button
              :pressed="true"
              variant="outline-info"
              size="lg"
              class="button-temp"
            >
              <b-img
                alt="weather"
                :src="urlImage(this.icon)"
                v-if="this.getIcon === true"
              ></b-img>
              <span class="temp" v-if="this.temperature > 0">
                {{ this.temperatureFormat(this.temperature) }}°C
              </span>
              <span class="temp" v-else>0°C</span>
            </b-button>
          </b-col>
        </b-row>
        <b-row class="text-center">
          <b-col>
            <h3 class="weather2" v-if="this.weather !== ''">
              {{ this.weatherDescription.toUpperCase() }}
            </h3>
          </b-col>
        </b-row>
      </div>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Weather",
  data() {
    return {
      api_key: "e12fa74525c1b00e0a3c27e1d46f156a",
      url_base: "http://api.openweathermap.org/data/2.5/",
      cors: "https://cors-anywhere.herokuapp.com/",
      query: "",
      city: "",
      country: "",
      temperature: 0,
      weather: "",
      weatherDescription: "",
      icon: "",
      getIcon: false,
      mainProps: {
        blank: true,
        blankColor: "#777",
        width: 75,
        height: 75,
        class: "m1",
      },
    };
  },
  methods: {
    runApi() {
      const url = `${this.cors}${this.url_base}weather?q=${this.query}&APPID=${this.api_key}`;
      axios.get(url).then((res) => {
        this.city = res.data.name;
        this.country = res.data.sys.country;
        this.temperature = res.data.main.temp;
        this.weather = res.data.weather[0].main;
        this.weatherDescription = res.data.weather[0].description;
        this.icon = res.data.weather[0].icon;
        this.getIcon = true;
      });
    },
    dateFormat() {
      let today = new Date();
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const days = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
      ];

      let day = days[today.getDay()];
      let date = today.getDate();
      let month = months[today.getMonth()];
      let year = today.getFullYear();

      return `${day}, ${month} ${date}, ${year}`;
    },
    temperatureFormat(temperature) {
      // Kelvin to Celcius
      return Math.round(temperature - 273.15);
    },
    urlImage(icon) {
      if (this.getIcon === true)
        return `http://openweathermap.org/img/wn/${icon}@2x.png`;
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
  outline: none;
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
  width: 20%;
  margin-top: 3%;
}

.temp {
  font-size: 50px;
  color: azure;
}

.weather2 {
  color: azure;
  font-size: 40px;
  margin-top: 3%;
}
</style>