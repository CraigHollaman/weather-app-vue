<template>
  <div id="app">
   <main :class=" getTime() > 11 ? 'night' : ''">
   <div class="search-box">
     <input type="text" 
     name="search" 
     class="search-bar" 
     placeholder="Enter a place..."
     v-model="query"
     v-on:keypress="getWeather" />
   </div>
   <div class="weather-holder" v-if="typeof weather.main != 'undefined'">
     <div class="location"> {{weather.name}} </div>
     <div class="date"> {{dateBuilder()}}</div>
     <div class="temperature">{{Math.round(weather.main.temp)}}°C</div>
    <div class="temperature-sub">
      Min.{{Math.round(weather.main.temp_min)}}°C 
      | 
      Max.{{Math.round(weather.main.temp_max)}}°C
    </div>
  <div class="conditions">{{weather.weather[0].description}}</div>
  <!-- <div>{{weather.weather[0].id}}</div> -->
  </div>
   </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '60d92afe4a38cfd33fb59984d761c987',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    getWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
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
    },
    getTime () {
      let d = new Date()
      let h = d.getHours()
      return h

    }
  }
}
</script>