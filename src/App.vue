<template> 
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 25 ? 'cool' : ''">
    <main >
      <div class="search" > 
        <input 
            type="text" 
            placeholder="Search..." 
            class="search__box"
            v-model="query"
            @keyup.enter="fetchWeather"
            @load="fetchWeatherBacGiang"
        >
      </div>
      <div class="title">
        <h3>A simple weather app created in VueJs</h3>
      </div>
      <div class="infor" v-if="typeof weather.main != 'undefined'"  >
          <div class="location-box">
              <div class="location">{{weather.name}}, {{weather.sys.country}} </div>
              <div class="date">{{dayBuilder()}}</div>
          </div>
          <div class="weather-box">
            <div class="temp"> {{Math.round(weather.main.temp)}}°C</div>
            <div class="weather"> {{weather.weather[0].main}}</div>
          </div>
      </div>
    </main>
  </div>
</template>

<script>
  import axios from 'axios'
export default {
  name: 'App',
  data () {
    return{
      api_key: '6feeebd8d0fa4fc4a915665c657e32af',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      query2: 'Bac Giang',
      weather: {},
     
    }
  },
  mounted () {
    this.fetchWeatherBacGiang()
  },
  methods :{
      fetchWeather () {
              // fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
              // .then(res =>{
              //   return res.json()
              // })
              // .then(e =>{this.weather = e})
              axios
                .get(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
                .then((res) => {
                    this.weather = res.data
                })
                // eslint-disable-next-line no-unused-vars
                .catch(error =>{ 
                  // console.log( "da bi loi" )
                  // console.log(error)
                  alert("Tên Tỉnh,Thành không đúng")
                   this.query = ""
                  })

      },
      fetchWeatherBacGiang () {
        axios
          .get(`${this.url_base}weather?q=${this.query2}&units=metric&APPID=${this.api_key}`)
          .then((e) =>{ this.weather = e.data})
      },
      dayBuilder () {
        let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
        let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
        let d = new Date()
        let date = d.getDate()
        // console.log(date) //23
        let day = days[d.getDay()]
        // console.log(day) //Sunday
        let month = months[d.getMonth()]
        let year = d.getFullYear()
        return `${day}, ${date} ${month} ${year}`

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
  font-family: 'Montserrat', sans-serif;
  
}
#app{
  background: url('./assets/cold-bg.jpg');
  background-position: bottom;
  background-size: cover;
  transition: .4s;
}
#app.cool{
  background: url('./assets/warm-bg.jpg');
  background-position: bottom;
  background-size: cover;
  transition: .4s;
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient( to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
/* .search */
.search{
  width: 100%;
  margin-bottom: 30px;
}
.search .search__box{
  display: block;
  width: 100%;
  padding: 15px 25px;
  border-top-left-radius: 20px;
  border-bottom-right-radius: 20px;
  border: none;
  outline: none;
  font-weight: 900;
  background: rgba(255, 255, 255, 0.35);
  transition: .4s all;
}
.search .search__box:focus{
  background-color: rgba(255, 255, 255, 0.7);
  border-bottom-left-radius: 20px;
  border-top-right-radius: 20px;
  border-top-left-radius: 0;
  border-bottom-right-radius: 0;
  box-shadow: 3px 4px 4px 0px rgba(0, 0, 0, 0.37);
}
/* .title */
.title h3{
  display: block;
  position: relative;
  text-align: center;
  color:red;
  text-transform: uppercase;
  padding: 5px;
  margin: 10px;
  text-shadow: 2px 2px 2px black;
}
/* .location */
.location-box .location{
  text-align: center;
  font-size: 30px;
  font-weight: 900;
  color: #fff;
  text-shadow: 1px 1px 1px rgba(0,0,0,0.75);
}
.location-box .date{
  text-align: center;
  font-size: 20px;
  font-style: italic;
  color: #fff;
}
/* .weather-box */
.weather-box{
  text-align: center;
}
.weather-box .temp {
  font-weight: 900;
  color: #fff;
  font-size: 50px;
  background:rgba(255, 255, 255, 0.55);
  display: inline-block;
  padding: 30px;
  border-radius: 15px;
  text-shadow: 1px 1px 1px rgba(0,0,0,0.75);
  box-shadow: 3px 4px 4px 0px rgba(0, 0, 0, 0.37);
  margin: 10px;
}
.weather-box .weather {
  font-size: 30px;
  color: #fff;
  font-weight: 900;
  font-style: italic;
}
</style>
