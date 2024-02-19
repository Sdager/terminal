<template >
  <main>

    <section class="weather">

      <div class="left">
        <div class="icon">
          <img v-if="weatherData" :src="weatherIcons[weatherData.weather[0].icon]" alt="Weather Icon">
        </div>
        <div class="temp">{{ weatherData ? (weatherData.main.temp - 273.15).toFixed(1) + '°C' : '' }}</div>
      </div>
      <div class="right">
        <div class="info1">
          <div class="time">{{ time }}</div>
        </div>
        <div class="info2">
          <div class="wind">Ветер: {{ weatherData ? weatherData.wind.speed + 'м/c' : '' }}</div>

          <div class="weather-description">{{ weatherData ? weatherData.weather[0].description : '' }}</div>
        </div>
      </div> <video src="sky.mp4" autoplay muted loop></video>
    </section>


    <section class="practice">
      <h1>Расписание <br>
        Производственной практики {{ page + 1 }}/{{ pagesCount }}</h1>

      <div class="card" v-for="(item, index) in displayFlex" :key="index">

        <div class="group">{{ item.groupTitle }}</div>
        <div class="spec">{{ }}</div>
        <div class="date_start">{{ new Date(item.started).toLocaleDateString('ru-RU') }}</div>

        <div class="date_end">{{ new Date(item.ended).toLocaleDateString('ru-RU') }}</div>
        <div class="fio">{{ item.userName }}</div>

      </div>

    </section>


  </main>
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
      time: null,
      weatherIcons: {
        "01d": "https://openweathermap.org/img/wn/01d.png",
        "01n": "https://openweathermap.org/img/wn/01n.png",
        "02d": "https://openweathermap.org/img/wn/02d.png",
        "02n": "https://openweathermap.org/img/wn/02n.png",
        "03d": "https://openweathermap.org/img/wn/03d.png",
        "03n": "https://openweathermap.org/img/wn/03т.png",
        "04d": "https://openweathermap.org/img/wn/04d@2x.png",
        "04n": "https://openweathermap.org/img/wn/04n.png",
        "09d": "https://openweathermap.org/img/wn/09d.png",
        "10d": " https://openweathermap.org/img/wn/10d.png",
        "10n": "https://openweathermap.org/img/wn/10n.png",
        "11d": "https://openweathermap.org/img/wn/11d.png",
        "13d": "https://openweathermap.org/img/wn/13d.png",
        "13n": "https://openweathermap.org/img/wn/13n.png",
        "50d": "https://openweathermap.org/img/wn/50d.png",
        "50n": "https://openweathermap.org/img/wn/50n.png"

      }

    }
  },
  mounted() {
    let app = this;
    setInterval(() => {
      let hoursMinute = new Date().toLocaleTimeString('RU-ru');
      //this.noSec = hoursMinute.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      app.time = hoursMinute;

    }, 1000);
    this.weather();
    this.practice();
    //this.dlina(); this.slide();
    setInterval(this.weather, 1800000);
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

    timeRefresh() {


    },
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

      let token = '31f450c397fb79d1be041ca49ecb1098';
      fetch(`https://api.openweathermap.org/data/2.5/weather?lat=55.737953&lon=37.741621&appid=${token}`


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

  user-select: none !important;
}
main {
  font-family: Arial, Helvetica, sans-serif;

}

video {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
  width: 100%;
  height: 100%;
  object-fit: cover;


}

.left {
  z-index: 10;
  width: 400px;
  border-radius: 30px;
  background-color: rgba(75, 172, 246, 0.78);

}

.right {
  z-index: 10;
  display: flex;
  flex-direction: column;
  gap: 50px;
  padding: 0;

}

.temp {
  font-size: 65px;
  font-weight: 600;
}

.info1 {
  width: 600px;
  height: 180px;
  border-radius: 30px;
  background-color: rgba(75, 172, 246, 0.78);
  border-radius: 40px;
  font-size: 100px;

}

.info2 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 180px;
  width: 600px;
  border-radius: 30px;
  background-color: rgba(75, 172, 246, 0.78);
  font-weight: 700;
  font-size: 43px;
}

img {
  width: 210px;
}

.info1 {}

section.weather {

  height: 300px;
  border-radius: 44px;
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  padding: 50px;
  box-shadow: 0px 0px 8px 0px rgba(34, 60, 80, 0.2);

  overflow: hidden;

  position: relative;
}

* {
  overflow: hidden;
}

h1 {
  font-size: 60px;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 500;

  background: #8bbeee
}

main {

  display: flex;
  justify-content: center;
  flex-direction: column;
  gap: 10px;
  overflow: hidden;
  padding: 25px;
  margin: 0px;
}

.card {
  gap: 20px;
  font-size: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 450px;
  height: 375px;
  background-color: rgb(231, 231, 231);
  border: 3px solid black;
  border-radius: 25px;
  box-shadow: 5px 5px 5px black;
}

.practice {
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
  margin: 0;
}</style>
