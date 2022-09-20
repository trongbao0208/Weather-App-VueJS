<template>
  <div class="my-5 mx-auto border border-2 w-50 border-dark h-50" >
    <div class="input-group mb-3">
      <input 
      type="text" 
      v-model="searchCityName" 
      class="form-control" 
      placeholder="Enter a city's name"
      @keyup.enter="getWeather"
      >
      
      <div class="input-group-append">
        <button class="btn btn-primary" type="button" id="button-addon2" @click="getWeather">Search</button>
      </div>
    </div>
    <div>
      <h1 class="text-center my-0 fw-bold" > {{titleCityName}}</h1>
      <p class="fs-1 text-center my-0 ">{{temperature}}</p>
      <img v-if="iconImage" :src="iconImage" alt="iconImage" class="img-fluid rounded mx-auto d-block">
      <p class="fs-2 text-center my-0">{{ mainDescription }}</p>
      <p class="fs-4 text-center my-0">{{ subDescription }}</p>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  components: {
    
  },
  data() {
    return {
      searchCityName: "",
      titleCityName: "------",
      temperature: null,
      mainDescription: null,
      subDescription: null,
      iconImage: null
    }
  },
  methods: {
    getWeather: function() {
      const apiUrl = 'https://api.openweathermap.org/data/2.5/weather';
      const key = "4a2d7dc29342a45a791dcccaca5a4864";
      axios
        .get(`${ apiUrl }?q=${this.searchCityName}&units=metric&appid=${key}`)
        .then(response => {
        this.titleCityName = response.data.name;
        this.temperature = `${response.data.main.temp}\u00B0 C`;
        this.mainDescription = response.data.weather[0].main;
        this.subDescription = response.data.weather[0].description;
        this.searchCityName = null;
        const iconcode = response.data.weather[0].icon;
        this.iconImage = `http://openweathermap.org/img/w/${iconcode}.png`
        })
        .catch(error => {
          this.titleCityName = "No city found";
          console.log(error);
          this.searchCityName = null;
          this.temperature = null;
          this.mainDescription = null;
          this.subDescription = null;
          this.iconImage = null;
        })

    },
  }
}
</script>

<style scoped>

</style>>


