<template>
  <div id="app" :class="backGroundWarm">    
          <div class="main overlay">
          <div class="container">

            <div class="search-box">
                <input class="search-bar"
                 type="text" v-model="query"
                 @click="searchInLowerCase" 
                 @keypress="fetchData" 
                 placeholder="Search Location..." 
                />

            </div>

                <div class="weather-wrap" v-if="weather.main">
                  <div class="location-box" v-if="(typeof weather.main != 'undefined')">
                    <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
                    <div class="date">{{dateBuilder()}}</div>

                  </div>
                  <div class="weather-box">
                    <div class="temp">{{Math.round(weather.main.temp)}}</div>
                    <div class="weather">{{weather.weather[0].main}}</div>
                    <div class="weather">{{weather.wind.speed}}</div>

                  </div>
                </div>
            </div>
        </div>

</div>
 

</template>

<script>




export default {
  name: 'App',
  data(){
    return{
      api_key: '5d9ada699e9024501c54a266276c5d35',
      ulr_base:'http://api.openweathermap.org/data/2.5/',
      query: "",
      weather: {}
    }
  },

  computed: {
  searchInLowerCase() {
    return this.query.toLowerCase().trim();
  },


  backGroundWarm(){
    return { warm: typeof this.weather.main != 'undefined' && this.weather.main.temp > 16 ? 'warm' : '' };

  }
},
  

  methods:{
    fetchData(e){
      if(e.key == "Enter" ){
 
        fetch(`${this.ulr_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        
       .then(res =>  res.json()
       ).then(this.setResult);

      }

    },

    setResult(result){
      this.weather = result
    },

    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }


  }


</script >

<style>
  @import '../src/assets/css/main.css';
  @import '../src/assets/css/normaliz.css';

    
</style>



