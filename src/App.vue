<template>
  <main>
    <div class="op">
      <div class="adress">Басовская 12</div>
      <div class="time">{{ time }}</div>
      <div class="date">{{ dater }}</div>
    </div>
    <section class="weather"><video src="back.gif.mp4" autoplay muted loop></video>
      <div class="top">
        <div class="left">
          <div class="now">
            Сейчас
            <div class="icon">
              <img class="weather_icon"
                v-if="weatherData"
                :src="weatherIcons[weatherData.weather[0].icon]"
                alt="Weather Icon"
              />
            </div>
          </div>
          <div class="temp">
            {{
              weatherData
                ? (weatherData.main.temp - 273.15).toFixed(1) + "°C"
                : ""
            }}
          </div>
        </div>
        <div class="right">
          <div class="weather-description">
            {{ weatherData ? weatherData.weather[0].description : "" }}
          </div>
          <div class="weather_feels_like">
            Ощущается как
            {{
              weatherData
                ? (weatherData.main.feels_like - 273.15).toFixed(1) + "°C"
                : ""
            }}
          </div>
        </div>
      </div>
      <div class="bottom">
        <div class="bottom_up">
          <div class="wind">
            <img  src="wind.png" alt="">
            {{ weatherData ? weatherData.wind.speed + "м/c" : "" }}
          </div>
          <div class="humidity">
            <img src="kapel.png" alt="">
            {{ weatherData ? weatherData.main.humidity + "%" : "" }}
          </div>
        </div>
        <div class="bottom_down">
              <div class="time_day">Максимальная: <br>{{
              weatherData
                ? (weatherData.main.temp_max - 273.15).toFixed(1) + "°C"
                : ""
            }}</div>
              <div class="time_day">Средняя: <br>{{
              weatherData
                ? (weatherData.main.temp - 273.15).toFixed(1) + "°C"
                : ""
            }}</div>
              <div class="time_day">Минимальная: <br>{{
              weatherData
                ? (weatherData.main.temp_min - 273.15).toFixed(1) + "°C"
                : ""
            }}</div>
                
        </div>
      </div>

      
    </section>

    <section class="practice">
      <h1 style="font-size: 40px">
        Расписание <br />
        Производственной практики {{ page + 1 }}/{{ pagesCount }}
      </h1>

      <div class="card" v-for="(item, index) in displayFlex" :key="index">
        <div class="group">{{ item.groupTitle }}</div>
        <div class="spec">{{}}</div>
        <div class="date_start">
          {{ new Date(item.started).toLocaleDateString("ru-RU") }}
        </div>

        <div class="date_end">
          {{ new Date(item.ended).toLocaleDateString("ru-RU") }}
        </div>
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
      dater: null,
      weatherIcons: {
        "01d": "https://openweathermap.org/img/wn/01d.png",
        "01n": "https://openweathermap.org/img/wn/01n.png",
        "02d": "https://openweathermap.org/img/wn/02d.png",
        "02n": "https://openweathermap.org/img/wn/02n.png",
        "03d": "https://openweathermap.org/img/wn/03d.png",
        "03n": "https://openweathermap.org/img/wn/03n.png",
        "04d": "https://openweathermap.org/img/wn/04d@2x.png",
        "04n": "https://openweathermap.org/img/wn/04n.png",
        "09d": "https://openweathermap.org/img/wn/09d.png",
        "10d": " https://openweathermap.org/img/wn/10d.png",
        "10n": "https://openweathermap.org/img/wn/10n.png",
        "11d": "https://openweathermap.org/img/wn/11d.png",
        "13d": "https://openweathermap.org/img/wn/13d.png",
        "13n": "https://openweathermap.org/img/wn/13n.png",
        "50d": "https://openweathermap.org/img/wn/50d.png",
        "50n": "https://openweathermap.org/img/wn/50n.png",
      },
    };
  },
  mounted() {
    let app = this;
    setInterval(() => {
      let hoursMinute = new Date().toLocaleTimeString("RU-ru");
      //this.noSec = hoursMinute.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      app.time = hoursMinute;
      let dater = new Date().toLocaleDateString("Ru-ru");
      app.dater = dater;
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
    timeRefresh() {},
    practice() {
      let app = this;
      fetch("https://api-crm.kioskapi.ru/api/groupPractice/8", {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
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

      let token = "31f450c397fb79d1be041ca49ecb1098";
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?lat=55.737953&lon=37.741621&appid=${token}`
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          app.weatherData = data;
          console.log(data);
        });
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
  },
};
</script>

<style>
.bottom .img{
  width: 35px;
}
.bottom_down{
  padding-top: 20px;
  display: flex;
  flex-direction: row;
  justify-content: center;
font-size: 25px;
color: white;
gap: 100px;
}
.bottom_up
{
  border-bottom:2px solid white ;
  gap: 60px;
  display: flex;
  flex-direction: row;
  font-size:25px ;
  color: white;
  padding-bottom: 15px;
}
.weather_feels_like {
  color: #a7b9ff;
}
.top {
  z-index: 10;
  display: flex;
  flex-direction: row;
  gap: 50px;
}
.bottom {
  z-index: 10;
}
* {
  user-select: none !important;
}
.time {
  font-size: 50px;
}
.adress {
  font-size: 30px;
}
.date {
  font-size: 30px;
}
.op {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  color: white;
  align-items: center;
  padding-bottom: 20px;
}
main {
  font-family: Arial, Helvetica, sans-serif;
  display: flex;
  justify-content: center;
  flex-direction: column;
  gap: 10px;
  overflow: hidden;
  padding-right: 80px;
  padding-left: 80px;
  padding-top: 40px;
  margin: 0px;
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
  display: flex;
  flex-direction: row;
  color: white;
  align-items: center;
}
.now {
  font-size: 30px;
}
.right {
  z-index: 10;
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: white;
  font-size: 30px;
  font-weight: 500;
  align-items: flex-start;
}

.temp {
  padding-top: 30px;
  font-size: 50px;
  font-weight: 700;
}

.weather_icon {
  width: 160px;
  padding: 0px;
}

section.weather {
  height: 300px;
  border-radius: 44px;
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 50px;
  box-shadow: 0px 0px 8px 0px rgba(34, 60, 80, 0.2);

  overflow: hidden;

  position: relative;
}

* {
 
}

h1 {
  font-size: 60px;
  color: white;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 500;
  display: flex;
  justify-content: center;
  text-align: center;
  margin: 0;
  background-image: url(assets/phine.png);
}

.card {
  gap: 20px;
  font-size: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 430px;
  height: 340px;
  background-color: rgb(231, 231, 231);
  border: 3px solid black;
  border-radius: 25px;
  box-shadow: 5px 5px 5px black;
  /* border-radius: 20px;
fill: linear-gradient(135deg, rgba(255, 255, 255, 0.5) 0%, rgba(255, 255, 255, 0) 100%), rgba(167, 185, 255, 0.24);
backdrop-filter: blur(50px); */
}

.practice {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  text-align: center;
  gap: 40px;
}
</style>
