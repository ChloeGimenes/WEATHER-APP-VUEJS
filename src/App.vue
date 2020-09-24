<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ?
    'warm' : '' && weather.weather[0].main == 'rain' ?
    'rain' : ''" >
    <main>
   
      <div class="search-box">
        <input 
        spellcheck="false"
        type ="text"
        class="search-bar"
        placeholder="weather forecast for..."
        v-model="query"
        @keypress="fetchWeather"
        onfocus="blur();"
         />
       </div>


        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">

        <div class="date">
           {{ dateBuilder()}}
        </div>

        <div id="right-box-second-flex">
         <div class="location-box">
          <div class="location">
           {{ weather.name }}, {{ weather.sys.country}}
          </div>
        </div>
        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp)}}°c
          </div>
        </div>
      </div>  
         
         <div id="left-box">
             <div id="description">  {{weather.weather[0].description}}</div>
              <div 
              id="pic"
              :class="typeof weather.main != 'undefined' && weather.weather[0].icon === '03d'|| weather.weather[0].icon === '03n'?
          'clouds' : '' || 
          weather.weather[0].icon === '04d'|| weather.weather[0].icon === '04n' ?
          'broken-clouds' : '' || 
          weather.weather[0].icon === '10d'|| weather.weather[0].icon === '10n' ?
          'rain' : '' || 
          weather.weather[0].icon === '01d'|| weather.weather[0].icon === '01n' ?
          'sun' : '' || 
          weather.weather[0].icon === '50d'|| weather.weather[0].icon === '50n' ?
          'mist' : '' || 
          weather.weather[0].icon === '13d'|| weather.weather[0].icon === '13n' ?
          'snow' : '' || 
          weather.weather[0].icon === '11d'|| weather.weather[0].icon === '11n' ?
          'storm' : '' || 
          weather.weather[0].icon === '02d'|| weather.weather[0].icon === '02n' ?
          'few-clouds' : '' || 
          weather.weather[0].icon === '09d'|| weather.weather[0].icon === '09n' ?
          'shower-rain' : '' "
          >
          </div>
        </div>
        
<!-- 
          <div class="weather">
            icone: {{weather.weather[0].icon}}
          </div> -->
        </div>

           <div id="info"  v-if="typeof weather.main != 'undefined'">
            <div class="info-box">
              <div class="icon">MIN {{Math.round(weather.main.temp_min)}}° </div>
              <div class="icon">MAX {{Math.round(weather.main.temp_max)}}° </div> 
            </div >
           <div class="info-box">
              <div class="icon" > SUNRISE {{new Date(weather.sys.sunrise*1000).toLocaleTimeString("en-GB").slice(0,5)}} </div>
              <div class="icon" > SUNSET {{new Date(weather.sys.sunset*1000).toLocaleTimeString("en-GB").slice(0,5)}} </div>
           </div>
           <div class="info-box">
              <div class="icon"> HUMIDITY {{weather.main.humidity}} % </div>
            <div class="icon"> WIND SPEED {{weather.wind.speed}} m/s</div>
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
      api_key: process.env.VUE_APP_API_KEY,
      url_base : 'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather: {},
     
     
  }
 },
 methods: {

   fetchWeather (e) {
     if (e.key == "Enter") {
       fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        })
        .then(this.setResults);
     }
   },

   setResults (results) {
     this.weather = results;
     console.log(results)
   },

   dateBuilder () {
     let d = new Date ();
     let months = [
       "January", "February", "March", "April", "May", "June", "July", "August", "Septembre", "Octobre", "November", "December"
     ];
     let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
   
    let day = days[d.getDay()];
    let date = d.getDate();
    let month = months[d.getMonth()];
    let year = d.getFullYear();

    return `${day}` + Array(10).fill('\xa0').join('') + ` ${date}` + Array(10).fill('\xa0').join('') + `${month}` + Array(10).fill('\xa0').join('') + `${year}`;
   },

  
 },



}

</script>

<style>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  }

  body {
    /* font-family: 'montserrat', sans-serif; */
    font-family: 'Poppins', sans-serif;
    color: white;

  }

  #left-box {
    display: flex;
    flex-direction: row;
    align-content: center;
  }

  #right-box {
    display: flex;
    flex-direction: row;
    align-items: center;
  }

  #right-box-second-flex {
  display: flex;
    flex-direction: column;
    width: 750px;;

  }

  #app {
    /* background-image: url('./assets/cold-bgg.png'); */
    background-size:cover;
    background-position: bottom;
    transition: 0.4s;
    }
  

/* PICTURES ///////////*/  
  #pic {
     width: 400px;
    height: 400px;
    -webkit-filter: grayscale(100%); /* Safari 6.0 - 9.0 */
    filter: grayscale(100%);
    background-size:cover;
  }
  
  .rain {
    background-image: url('./assets/lightrain.png');  
  }
.mist {
    background-image: url('./assets/mist.png'); 
  }
.clouds {
    background-image: url('./assets/clouds.png');
}  
 .broken-clouds {
    background-image: url('./assets/broken-clouds.png');
    opacity:  0.8; 
 }       
 .sun {
    background-image: url('./assets/sun.png');
}
 .snow {
    background-image: url('./assets/snow.png'); 
 } 
 .storm{
    background-image: url('./assets/storm.png');  
 } 
  .few-clouds{
    background-image: url('./assets/few-clouds.png');
 }   
 .shower-rain {
    background-image: url('./assets/shower-rain.png');
 } 

/* OTHERS /////////////*/

  main {
    min-height: 100vh;
    padding: 25px;
    background-color: rgb(26, 26, 27);
    /* background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75)); */
    }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar {
    font-family: 'Poppins', sans-serif;
    color: white;
    display: block;
    width: 30%;
    padding: 15px;
    text-align: center;
    font-size: 20px;
    margin: auto;
    margin-top: 4%;
    margin-bottom: 6%;
    
    appearance: none;
    border: none;
    outline: none;
    background: none;
    cursor: pointer;
    border: 0;
    outline: 0;
    border-bottom: 2px solid rgb(253, 253, 253);

    /* box-shadow: 0px 0px 8px rgba(0,0,0,0.25); */
    /* background-color:  rgba(255, 255, 255, 0.5); */
    border-radius: 0px 16 0px 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus {
    border-bottom: 2px solid rgb(205, 219, 0);
  }
  
  .location-box .location {
    color: rgb(245, 245, 245);
    font-size: 65px;
    font-weight: 700;
    text-align: center;
    /* text-shadow: 1px 3px rgba(0,0,0,0.25); */
  }

.date {
    align-self: center;
    color: rgb(205, 219, 0);
    font-size: 22px;
    writing-mode: vertical-rl;
  }

  .weather-wrap {
    display: flex;
    flex-direction: row-reverse;
    justify-content: space-evenly;
    margin-top: 15px;
  }
  
  .weather-box {
    text-align: center;
  }

  .weather-box .temp {
    color: rgb(245, 245, 245);
    font-size: 250px;
    margin: -45px;
  }

  .weather-box .weather {
    color: rgb(10, 54, 34);
    font-size: 25px;
    /* text-shadow: 3px 6px rgba(0,0,0,0.25); */
  }

  #info {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    margin-top: 3%;
    
  }

  #description {
    font-size: 35px;
    margin-right: 10px;
    writing-mode: vertical-rl;
  }

/* MOBILE /////////////////////////////////////*/
@media  (max-width: 850px) {
  
    .search-box .search-bar {
      margin-top: 15%;
      width: 90%;
      text-align: center;
     font-size: 15px;
    }
    .weather-wrap {
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    margin-top: 15px;
    }
    .weather-box .temp {
    font-size: 110px;
    margin: inherit;
    }
    #pic{
    justify-content: center;
    align-content: inherit;
    max-width: 200px;
    max-height: 200px;
    object-fit: cover;
   }
   .date {
    align-self: center;
    color: rgb(205, 219, 0);
    font-size: 14px;
    writing-mode: inherit;
    margin-top: 10px;
    margin-bottom: 20px;
   }
   #right-box-second-flex{
    width: 300px;
   }
   .location-box .location {
    font-size: 20px;
    text-transform: uppercase;
    line-height: 38px;
    justify-content: left;
    width: fit-content;
   }


   .icon {
     margin-top: 15px;
     margin-bottom: 25px;;
   }
   #left-box {
    margin-bottom: 17px;
   }
   #info {
     font-size: 13px;
   }
   .info-box {
     margin-left: 15px;
   }
 }

/* TABLET //////////////////////////////////*/
@media screen and (min-width: 850px) and (max-width: 1500px) { 
  main {
    padding: 45px;
  }
  #pic {
    width: 300px;
    height: 300px;
  }
  .search-box .search-bar {
    margin-top: 10%;
    width: 90%;
    text-align: center;
    font-size: 30px;
    }
  .location-box .location {
    font-size: 40px;
    margin-top: 30px;
    text-transform: uppercase;
   }
  .weather-box .temp {
    font-size: 140px;
    margin: inherit;
  }
  .weather-wrap {
    margin-top: 10%;
  }
}

</style>
