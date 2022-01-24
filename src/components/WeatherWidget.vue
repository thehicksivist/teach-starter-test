<template>
  <div class="container">
      <div class="widget">
        <div class="top">
          <h3>{{ msg }}</h3>
          <select name="city" @change="getData()" class="form-control" v-model="city">
            <option default value="" >
              ---Select a city to view the current weather---
            </option>
            <option value="brisbane">
              Brisbane
            </option>
            <option value="sydney">
              Sydney
            </option>
            <option value="melbourne">
              Melbourne
            </option>
          </select>
        </div>
        <div class="bottom">
          <h2 v-if="this.cityName !== ''">{{ this.cityName }}</h2>
          <div class="iconContainer">
            <div v-if="!this.isLoading && this.city !== ''" class="icon">
              <img class="my-icon" :src="this.icon">
            </div>
            <div>
              <h1 v-if="this.cityTemp !== ''" class="temp">{{ (Math.round(parseInt(this.cityTemp) * 100) / 100).toFixed(0) }}&#176;C</h1>
              <div>{{ this.description }}</div>
            </div>
          </div>
          <div v-if="this.isLoading" class="spinner"></div>
        </div>
      </div>
  </div>
</template>

<script>
  const apiKey = "8c022cba4b88660196e569c2e267ade0";

  export default {
    name: 'WeatherWidget',
    props: {
      msg: String,
    },
    data() {
      return {
        info: '',
        city: '',
        cityName: '',
        cityTemp: '',
        icon: '',
        description: '',
        isLoading: false
      };
    },
    methods: {

      resetValues() {
        this.cityName = '',
        this.cityTemp = '',
        this.icon = '',
        this.description = ''
      },

      async getData() {
        this.resetValues();
        if(this.city !== ''){
          this.resetValues();
          try {
            this.isLoading = true;
            const url = "https://api.openweathermap.org/data/2.5/weather?q=" + this.city + "&appid=" + apiKey + "&units=metric";
            const response = await fetch(url);
            const data = await response.json();
            if(response){
              setTimeout(() => {
                this.isLoading = false;
                this.cityName = data.name;
                this.cityTemp = data.main.temp;
                this.description = data.weather[0].description;
                this.icon = "http://openweathermap.org/img/wn/" + data.weather[0].icon + "@2x.png";
              }, 1000);
            }
          } catch (error) {
            this.isLoading = false;
            console.log(error);
          }
        }
      },
      
    },
  };
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    color: #ffffff;
  }
  h3 {
    padding-bottom: 10px;
  }
  select, select > * {
    color: #000000;
    width: 100%;
  }
  .container {
    width: 100%;
    height: 100vh;
    background-color: rgb(0, 153, 255);

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .top {
    padding: 30px 40px;
    background-color: #1f2325;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }
  .bottom {
    padding: 30px 40px;
    background-color: #40515d;
    min-height: 100px;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
  }
  .iconContainer {
    padding-top: 20px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
  }
  .icon {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background-color: rgb(0, 153, 255);
  }
  .spinner {
    width: 30px;
    height: 30px;
    padding: 20px;
    margin: 30px 30px 30px 95px;
    border: 3px solid rgba(161, 161, 161, 0.3);
    border-radius: 50%;
    border-top-color: rgb(0, 0, 0);
    animation: spin 1s ease-in-out infinite;
    -webkit-animation: spin 1s ease-in-out infinite;
    align-self: center;
  }
  .temp {
    font-size: 30px;
    font-weight: 600;
  }
  @keyframes spin {
    to { -webkit-transform: rotate(360deg); }
  }
  @-webkit-keyframes spin {
    to { -webkit-transform: rotate(360deg); }
  }

  @media only screen and (max-width: 400px) {
    .iconContainer {
      flex-direction: column;
    }
    .temp {
      margin-top: 20px
    }
  }
</style>
