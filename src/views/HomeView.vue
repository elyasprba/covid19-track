<template>
   <div class="home">
      <img alt="Vue logo" src="../assets/covid-img.jpg" />
      <h5>Last Update {{ lastUpdate }}</h5>
      <b-container class="bv-example-row">
         <b-form-select v-model="selected" class="form-select mb-3" @change="pickCountry">
            <b-form-select-option :value="null">Global</b-form-select-option>
            <b-form-select-option v-for="(item, index) in country" :value="item.iso3" :key="index">{{ item.name }}</b-form-select-option>
         </b-form-select>
         <b-row>
            <b-col cols="12" lg="4">
               <div class="card border-primary mb-3">
                  <div class="card-header text-primary">Confirmed</div>
                  <div class="card-body text-primary">
                     <h5>{{ confirmed }}</h5>
                     <p>Number of positive people</p>
                  </div>
               </div>
            </b-col>
            <b-col cols="12" lg="4"
               ><div class="card border-success mb-3">
                  <div class="card-header text-success">Recovered</div>
                  <div class="card-body text-success">
                     <h5>{{ recovered }}</h5>
                     <p>Number of people who are said to be cured</p>
                  </div>
               </div></b-col
            >
            <b-col cols="12" lg="4"
               ><div class="card border-danger mb-3">
                  <div class="card-header text-danger">Death</div>
                  <div class="card-body text-danger">
                     <h5>{{ death }}</h5>
                     <p>Number of people who died</p>
                  </div>
               </div></b-col
            >
         </b-row>
      </b-container>
   </div>
</template>

<script>
import axios from 'axios';

export default {
   name: 'HomeView',
   data() {
      return {
         selected: null,
         country: [],
         lastUpdate: '',
         confirmed: 0,
         recovered: 0,
         death: 0,
      };
   },
   created() {
      this.getCountry();
      this.getDataCovid();
   },
   methods: {
      getCountry() {
         axios
            .get('https://covid19.mathdro.id/api/countries')
            .then((res) => {
               // console.log(res.data.countries);
               this.country = res.data.countries;
            })
            .catch((err) => {
               console.log(err);
            });
      },
      getDataCovid(country) {
         let setUrl = '';

         if (country) {
            setUrl = 'https://covid19.mathdro.id/api/countries/' + country;
         } else {
            setUrl = 'https://covid19.mathdro.id/api/';
         }
         // console.log(setUrl);
         axios
            .get(setUrl)
            .then((res) => {
               // console.log(res);
               this.confirmed = res.data.confirmed.value;
               this.recovered = res.data.recovered.value;
               this.death = res.data.deaths.value;
               this.lastUpdate = res.data.lastUpdate;
            })
            .catch((err) => {
               console.log(err);
            });
      },
      pickCountry(event) {
         // console.log(event);
         this.getDataCovid(event);
      },
   },
};
</script>

<style scoped>
img {
   width: 250px;
   margin: 30px auto;
}
</style>
