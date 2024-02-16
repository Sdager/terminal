  <template >
  <div class="main">

  <h1>Расписание <br>
  Производственной практики</h1>
  <div class="all" style="color: red;" @click="practice()">

  <div class="card" v-for="(item, index) in displayFlex" :key="index">
    
    <div class="group">{{item.groupTitle  }}</div>
    <div class="spec">{{  }}</div>
    <div class="date_start">{{ new Date(item.started).toLocaleDateString('ru-RU') }}</div>
    <div class="date_end">{{ new Date(item.ended).toLocaleDateString('ru-RU') }}</div>
    <div class="fio">{{ item.userName }}</div>

  </div>
    
  </div>


  </div>


  </template>

  <script>
  export default{
    data() {
      return {
        items: [],
        started: null,
        ended: null,
        groupTitle: null,
        userName: null,
        page: 0
      }
    },
    mounted() {
      this.practice();
     // setInterval(this.practice, 300);
      setTimeout(() => {
        
      this.page++;
    }, 5000);
    },
    computed: {

      displayFlex: function() {

          let started = 8 * this.page;
          return this.items.slice(started, started + 8);
      }
    },
    methods: {
      async practice(){
        const response = await fetch('https://api-crm.kioskapi.ru/api/groupPractice/8',{
          method: 'GET',
          headers: {
            "Content-Type":"application/json"
          },
      
        } );
  let data = await response.json();
        this.items = data;
      

        if (this.page === 1) {
      this.page = 0; 
    } else {
      this.page++;}

  
    }
    }
  }
  </script>

  <style>
  .main{
    max-width: 1080px;
    max-height: 1980px;
  }
  .card{
  width: 500px;
  background-color: pink;
  border: 2px solid black;
  }
  .all{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    text-align: center;
    gap: 30px;
  }
  body{
    display: flex;
    justify-content: center;
    text-align: center;
  }
  </style>
