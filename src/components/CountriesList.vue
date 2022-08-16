<template>
  <div class="container">
    <h2 class="text-center my-3">Country list</h2>
    <div v-if="this.countries" class="row">
      <div class="col-4">
        <ul class="list-group">
          <router-link :to="`/list/${country.alpha3Code}`" v-for="(country, index) in countries" :key="index">
            <li class="list-group-item">
            <img class="country-flag " :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`" alt="">
            <p class="text-center fw-bold">{{country.name.common}}</p>
            </li>
          </router-link>
        </ul>
      </div>
      <div class="col-8">
        <router-view/>
      </div>
    </div>
    <div v-else >
      <spinnerComponent  :text="'Loading countries...'"/>
    </div>
    
  </div>
</template>

<script>
import spinnerComponent from'./SpinnerComponent.vue';

export default {
    name:'CountriesList',
    components:{
      spinnerComponent,
      },
    data(){
        return{
          countries:null,
        }
    },
    methods:{
     async fetchCuntries(){
        const res = await fetch('https://ih-countries-api.herokuapp.com/countries');

        const Countriesdata = await res.json();
        this.countries = Countriesdata.sort((a,b)=>{
          return a.name.common.localeCompare(b.name.common)
        })
      },
    
    },
    // we use created-hook to make the initial call to the db and get the info b4 displaying it
    created(){
      this.fetchCuntries();
    }


}
</script>

<style scoped>
.country-flag{
  width: 150px;
}

</style>