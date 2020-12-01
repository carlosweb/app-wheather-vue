<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''" >
   <main>
     <div class="search-box">
       <input type="text" 
       class="search-bar" 
       placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
       >
      
     </div>
     <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
       <div class="location-box">
         <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
         <div class="date"> {{ dateBuilder() }} </div>
       </div>

       <div class="weather-box">
         <div class="temp">{{ Math.round(weather.main.temp) }}ºC</div>
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
      API_KEY: 'dcfad62ba87de011b81d6237e54750bc',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&lang=pt_br&APPID=${this.API_KEY}`)
        .then(res => {
          return res.json()
        }).then(this.setResults)
      }
    },
    setResults (results) {
      this.weather = results
    },
    dateBuilder () {
      let d = new Date()
      let months = ["Janeiro", "Fevereiro", "Março","Abril", "Maio", "Junho","Julho", "Agosto", "Setembro","Outubro", "Novembro", "Dezembro",]
      let days = ["Domingo", "Segunda", "Terça", "Quarta","Quinta", "Sexta", "Sabado"]

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<style>

*{
margin: 0;
padding: 0;
  box-sizing: border-box;
}

#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: all .3s ease;
}

#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
}

.search-box {
  width: 100%;
  margin-bottom: 30px; 
}

.search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.10);
    background-color: rgba(255, 255, 255, 0.8);
    border-radius: 0.4rem;
  }

  .location-box .location {
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date {
    color: white;
    font-size: 20px;
    font-weight: 300;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .weather-box{
    text-align: center;
  }
  
.weather-box .temp{
    display: inline-block;
    padding: 10px 25px;
    color: white;
    font-size: 100px;
    font-weight: 800;

    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 0.8rem;
    margin: 30px 0px;
  
}  

.weather-box .weather {
  color: #ffffff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}  


</style>
