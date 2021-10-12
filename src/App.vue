<template>
 <div id="app" :class="typeof weather.main !== 'undefined' && weather.main.temp > 16 ? 'warm' : 'cold'">
   <main>
     <div class="search-box">
       <input 
       type="text" 
       placeholder="Search..." 
       class="search-bar" 
       v-model="query"
       v-on:keypress="fetchWeather"
       />
     </div>

     <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
       <div class="location-box">
         <div class="location">{{ weather.name }}, {{ weather.sys.country}}</div>
         <div class="date">{{ dateBuilder() }}</div>
       </div>

       <div class="weather-box">
        <div class="temp">{{Math.round(weather.main.temp)}}°c <br> <span style="font-weight: 600; font-size: 18px; text-shadow: 1px 2px rgba(0, 0, 0, 0.25);">{{Math.round(weather.main.temp_min)}}°c ~ {{Math.round(weather.main.temp_max)}}°c</span></div>
        <div class="weather">{{ weather.weather[0].main }}</div>
     </div>
     </div>

     
   </main>
 </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: 'dd58d598e823907c6a63fb58a5c9fcea',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },

  methods: {

    fetchWeather (e) {
      if (e.key === "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`).then(res =>{
            return res.json();
        }).then(this.setResult)
        .catch(error => console.log("", error));
      }
    },

    setResult (result) {
      this.weather = result;
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
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'montserrat', sans-serif;
  }

  #app {
    margin: auto;
    background-size: cover;
    background-position: center;
    transition: 0.4s;
  }

  #app .warm {
    background-image: url('./assets/warm-bg.jpg') !important;
  }
  #app .cold {
    background-image: url('./assets/cold-bg.jpg') !important;
  }

  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.7));
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.07);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus {
    background-color: rgba(255, 255, 255, 0.75);
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.2);
    border-radius: 16px 0px 16px 0px;
  }

  .location-box .location {
    color:#ffffff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    margin-bottom: 10px;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date {
    color: #ffffff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }

  .weather-box {
    text-align: center;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #ffffff;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    
  }

  .weather-box .weather {
    color: #FFF;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
</style>
