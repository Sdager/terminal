<template >
  <div class="main">
    <h1>Расписание <br>
      Производственной практики {{ page + 1 }}/{{ pagesCount }}</h1>
    <div class="all" style="color: black;">
      <div class="card" v-for="(item, index) in displayFlex" :key="index">
        <div class="time">{{ noSec }}</div>
        <div class="group">{{ item.groupTitle }}</div>
        <div class="spec">{{ }}</div>
        <div class="date_start">{{ new Date(item.started).toLocaleDateString('ru-RU') }}</div>

        <div class="date_end">{{ new Date(item.ended).toLocaleDateString('ru-RU') }}</div>
        <div class="fio">{{ item.userName }}</div>
        <div class="temp">{{ }}</div>

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
      noSec: null

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

      let started = 8 * this.page;
      return this.items.slice(started, started + 8);
    },
    pagesCount: function () {
      let str = Math.floor(this.items.length / 8);
      if (this.items.length % 8 > 0) {

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



      fetch('https://api.openweathermap.org/data/2.5/weather?lat=55.75&lon=37.61&appid=31f450c397fb79d1be041ca49ecb1098'
      
       
      )
        .then(response => {
          if (response.ok) {
            return response.json();
          }

        })
        .then(data => {
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
