<script>
import axios from "axios";
import TimeCurrent from "@/components/Time.vue";

export default {
  components: {TimeCurrent},

  data() {
    return {
      url_base: 'https://api.open-meteo.com/v1/forecast',
      country: "",
      country_show: "",
      latitude: '',
      longitude: '',
      temperature: '',
    };
  },
  methods: {
    async getWeather() {
      if (this.country.length > 1) {
        let response = await axios.get(`https://geocoding-api.open-meteo.com/v1/search?name=${this.country}&count=1&language=en&format=json`);
        this.country_show = response.data.results[0]['name'];
        this.latitude = response.data.results[0]['latitude'];
        this.longitude = response.data.results[0]['longitude'];
        let weather = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${this.latitude}&longitude=${this.longitude}&current=temperature_2m`);
        this.temperature = weather.data.current['temperature_2m'];
      }
    },
    todayDate() {
      const months = [
        "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov",
        "Dec",];
      const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
      let d = new Date();
      let month = months[d.getMonth()];
      let day = days[d.getDay()];
      let date = d.getDate();
      let year = d.getFullYear();

      return `${month} ${date} ${day} ${year}`;
    }
  }
};
</script>

<template>
  <div class="weather-container">
    <div class="weather-wrap">
      <div class="search-box">
        <input type="text" placeholder="Search..." class="search-bar"
               v-model="country">
        <button @click="getWeather">Click</button>
      </div>

      <div class="weather-info">
        <div class="location-box">
          <div class="date">
            {{ todayDate() }}
            <div>
              <time-current></time-current>
            </div>
          </div>
          <div class="location">
            {{ this.country_show }}
          </div>
        </div>
        <div class="weather-box" v-if="temperature.length !== 0">
          <div class="temp">{{ this.temperature }}°c</div>
          <div class="weather"></div>
          <div class="icon">
          </div>
        </div>
      </div>


    </div>

  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;500;700;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat";
}

.weather-container {
  background-size: cover;
  background-position: center;
  transition: 0.4s;
  width: 375px;
  margin: 0 auto;
  border-radius: 25px;
  margin-top: 50px;
  box-shadow: 0 0 30px #00000065;
}

.weather-wrap {
  height: 600px;
  padding: 25px;
  border-radius: 25px;
  background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.15),
      rgba(0, 0, 0, 0.4)
  );
}

.search-box {
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: center;
}

.search-box .search-bar {
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
  transition: 0.4s;
}

.search-box button {
  width: 100%;
  height: 100%;
  margin-left: 3px;
  padding: 15px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  font-style: italic;
  text-align: center;
  margin-top: 30px;
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  margin-top: 30px;

}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 80px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-style: italic;
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
