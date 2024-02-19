<template >
  <div class="main">
    <h1>Расписание <br>
      Производственной практики {{ page + 1 }}/{{ pagesCount }}</h1>
      <div class="temp">{{weatherData ? (weatherData.main.temp - 273.15).toFixed(1) + '°C' : ''}}</div>
     <div class="wind">Ветер: {{ weatherData ? weatherData.wind.speed + 'м/c' : '' }}</div>
     <div class="icon">
  <img v-if="weatherData" :src="weatherIcons[weatherData.weather[0].icon]" alt="Weather Icon">
</div>
  <div class="weather-description">Осадки: {{ weatherData ? weatherData.weather[0].description : '' }}</div>
      <div class="main"></div>

    <div class="all" style="color: black;">
      
      <div class="card" v-for="(item, index) in displayFlex" :key="index">
        <div class="time">{{ noSec }}</div>
        <div class="group">{{ item.groupTitle }}</div>
        <div class="spec">{{ }}</div>
        <div class="date_start">{{ new Date(item.started).toLocaleDateString('ru-RU') }}</div>

        <div class="date_end">{{ new Date(item.ended).toLocaleDateString('ru-RU') }}</div>
        <div class="fio">{{ item.userName }}</div>
       
      </div>

    </div>


  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      started: null,
      ended: null,
      groupTitle: null,
      userName: null,
      page: 0,
      len: null,
      temp: null,
      noSec: null,
      weatherIcons: {
      "01d": "src/img/01d.svg",
      "01n": "src/img/01n.svg",
      "02d": "src/img/02d.svg",
      "02n": "src/img/02n.svg",
      "03d": "src/img/03d.svg",
      "03n": "src/img/03n.svg",
      "04d": ".src/img/04d.svg",
      "04n": "src/img/04n.svg",
     "09d":"src/img/09d.svg",
     "10d":" src/img/10d.svg",
     "10n": "src/img/10n.svg",
     "11d": "src/img/11d.svg",
     "13d": "src/img/13d.svg",
     "13n": "src/img/13n.svg", 
     "50d": "src/img/50d.svg",
     "50n": "src/img/50n.svg"

    }

    }
  },
  mounted() {
    
    this.weather();
    this.practice();
    //this.dlina(); this.slide();

    this.time();
    setInterval(this.slide, 5800);

    //setTimeout(() => {

    //  this.page++;
    //  this.slide();
    //}, 5000);
  },
  computed: {

    displayFlex: function () {

      let started = 6 * this.page;
      return this.items.slice(started, started + 6);
    },
    pagesCount: function () {
      let str = Math.floor(this.items.length / 6);
      if (this.items.length % 6 > 0) {

        str += 1;
      }
      return str;
    },
  },
  methods: {
    practice() {
      let app = this;
      fetch('https://api-crm.kioskapi.ru/api/groupPractice/8', {
        method: 'GET',
        headers: {
          "Content-Type": "application/json"
        },
      })
        .then((response) => response.json())
        .then((response) => {
          app.items = response;
          app.len = response.length;
          // app.dlina();
        });

    },

    weather() {
let app = this;


      fetch('https://api.openweathermap.org/data/2.5/weather?lat=55.75&lon=37.61&appid=31f450c397fb79d1be041ca49ecb1098'
      
       
      )
        .then(response => {
          if (response.ok) {
            return response.json();
          }

        })
        .then(data => {
          app.weatherData = data;
          console.log(data);
        })
        

    },



    time() {
      let hoursMinute = new Date();
      this.noSec = hoursMinute.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      return this.noSec;
    },
    //dlina() {
    //  if (this.len !== null) {
    //    console.log(this.len);
    //  }
    //},


    slide() {
      if (this.page + 1 >= this.pagesCount) {
        this.page = 0;
      } else {
        this.page++;
      }

    },

  }
}
</script>

<style>
* {
  overflow: hidden;
}

h1 {
  font-size: 60px;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 500;
  display: flex;
  justify-content: center;
  overflow: hidden;
  padding: 0px;
  margin: 0px;
  background: rgb(135, 206, 218);


}

.main {}

.card {
  gap: 20px;
  font-size: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 450px;
  height: 380px;
  background-color: rgb(245, 245, 220);
  border: 3px solid black;
  border-radius: 18px;
}

.all {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  text-align: center;
  gap: 40px;
}

body {
  display: flex;
  justify-content: center;
  text-align: center;
}
</style>
