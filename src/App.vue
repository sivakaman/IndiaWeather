<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 25 ? 'warm': ''">
    <main>
      <div class="search-bax">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..." 
          v-model="query"
          @keypress="fetchWeather"
          />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
            <div class="date">{{ dateBuilder() }}</div>
          </div>
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}*c</div>
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
    return{
      api_key: '6ec0a78efff1bdd65716db4a5f3169ac',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods:{
    fetchWeather (e){
      if (e.key == 'Enter'){
        // console.log("OK")
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
        .then(res => res.json() )
        .then(this.setResults)
      }
    },
    setResults (results){
      // console.log(results)
      this.weather = results
    },
    dateBuilder (){
      let d = new Date();
      // d.toLocaleString('en-US', { timeZone: this.weather.name })
      // console.log(d)
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
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: top;
  transition: 0.2s;
}

#app.warm{
  background-image: url('./assets/warm-bg.jpg');
  transition: 0.2s;
}

main{
  min-height: 100vh;
  padding: 1.5em;
  background-image: linear-gradient(to bottom, rgba(175, 166, 166, 0.25), rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 1.8em;
}

.search-bar{
  display: block;
  width: 100%;
  padding: 1.2em;
  color: #1f1e1e;
  font-size: 1.8em;
  appearance: none;
  border:none;
  outline: none;
  background: none;

  box-shadow: 0em 0em .5em rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0em 1em 0em 1em;
  transition: 0.4s;
}
::placeholder {
  color: #303030;
  opacity: 1; /* Firefox */
  font-weight: 530;
}

.search-bar:focus{
  box-shadow: 0em 0em 1em rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 1em 0em 1em 0em;
}

.location-box .location{
  color: #fff;
  font-size: 2em;
  font-weight: 500;
  text-align: center;
  text-shadow: 0.02em .08em rgba(0, 0, 0, 0.75);
  margin: 3em 0em 0em 0em ;

}

.location-box .date{
  color: #fff;
  font-size: 1.5em;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  /* text-shadow: 0.09em .1em rgba(0, 0, 0, 0.75); */
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 0.4em 0.8em;
  color:#fff;
  font-size: 4.6em;
  font-weight: 900;

  text-shadow: 0.02em .06em rgba(0, 0, 0, 0.75);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 0.4em;
  margin: 0.2em 0em;

  box-shadow: 0.03em 0.06em rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color: #fff;
  font-size: 2.5em;
  font-weight: 700;
  font-style: italic;
  text-shadow: 0.02em .06em rgba(0, 0, 0, 0.75);
  margin: 0em 0em;
}
</style>
