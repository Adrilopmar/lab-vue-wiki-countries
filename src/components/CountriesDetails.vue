<template>
  <!-- <div>
    <img
      :src="`https://flagcdn.com/w320/${alpha2Code.toLowerCase()}.png`"
      class="border"
      alt="country flag"
    />
    <h2>{{ name }}</h2>
    <ul class="list-group list-group-flush">
      <li class="list-group-item d-flex justify-content-between">
        <p class="fw-bold">Capital</p>
        <p class="me-5">{{ capital }}</p>
      </li>
      <li
        class="list-group-item list-group-item d-flex justify-content-between"
      >
        <p class="fw-bold">Area</p>
        <p class="me-5">{{ area }} km2</p>
      </li>
      <li class="list-group-item list-group-item d-flex flex-wrap">
        <h3 class="fw-bold w-100">Borders</h3>
        <p v-if="borders.length === 0">This country has no borders... yet...</p>
        <div v-for="(border, index) in borders" :key="index" class="me-5">
          <router-link :to="`/list/${border}`">{{ border }}</router-link>
        </div>
      </li>
    </ul>
  </div> -->
  <!-- using an object to store all data properties and use it on template -->
  <div v-if="countryInfo">
    <img
      v-bind:src="`https://flagcdn.com/w320/${countryInfo.alpha2Code.toLowerCase()}.png`"
      class="border"
      alt="country flag"
    />
    <h2>{{ countryInfo.name.common }}</h2>
    <ul class="list-group list-group-flush">
      <li class="list-group-item d-flex justify-content-between">
        <p class="fw-bold">Capital</p>
        <p class="me-5">{{ countryInfo.capital[0] }}</p>
      </li>
      <li
        class="list-group-item list-group-item d-flex justify-content-between"
      >
        <p class="fw-bold">Area</p>
        <p class="me-5">{{ countryInfo.area }} km2</p>
      </li>
      <li class="list-group-item list-group-item d-flex flex-wrap">
        <h3 class="fw-bold w-100">Borders</h3>
        <p v-if="countryInfo.borders.length === 0">This country has no borders... yet...</p>
        <div v-for="(border, index) in countryInfo.borders" :key="index" class="me-5">
          <router-link :to="`/list/${border}`">{{ border }}</router-link>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { computed, onMounted, reactive, ref,watch, } from "vue";
import { useRoute } from 'vue-router';

export default {
  name: "CountriesDetails",
  //setup(){} -> iniciación de composition API es el primero de todos los hooks ligecycle con el ref() podemos acceder a cualquier dato
  //ref() es un metodo reservado de VUEjs siempre se declaran en setup() y se tiene que devolver en el return{} de la misma.

  setup() {
    const name = ref("");
    const capital = ref("");
    const alpha3Code = ref("");
    const area = ref("");
    const borders = ref([]);
    const alpha2Code = ref("");
    const countryInfo = ref(null);

    // const countryData = reactive({
    //   name: "",
    //   capital: "",
    //   alpha3Code: "",
    //   area: "",
    //   borders: [],
    //   alpha2Code: "",
    // }),

    //const to use router inside the project
    const route = useRoute();

    const getCountryAlphaCode = async ()=>{
      const alpha3Code = route.params.alpha3Code;

      const res = await fetch(
        `https://ih-countries-api.herokuapp.com/countries/${alpha3Code}`
      );
      const finalRes = await res.json();

      const name = finalRes.name.common;
      const capital = finalRes.capital[0];
      const area = finalRes.area;
      const borders = finalRes.borders;
      const alpha2Code = finalRes.alpha2Code;

       countryInfo.value = finalRes

      return {name,capital,area,borders,alpha2Code,countryInfo};
    };

    

    // mounted hook converted into composition API
    onMounted(()=>{
      getCountryAlphaCode();
    });

  //boulerPlate to pass computed properties
    //computed(function(){})
    // const countryCode = computed(function(){
    //   return route.params.alpha3Code
    // });
    const countryCode = computed(()=>{
      return route.params.alpha3Code
    });

    // watch object transform into composition API
    watch(countryCode,(newCountryCode)=>{
      getCountryAlphaCode()
    })

    return {
      name,
      capital,
      alpha3Code,
      area,
      borders,
      alpha2Code,
      countryInfo,
      getCountryAlphaCode,
      countryCode,
    };

  },
  
  // data() {
  //   return {
  //     name: "",
  //     capital: "",
  //     alpha3Code: "",
  //     area: "",
  //     borders: [],
  //     alpha2Code: "",
  // passing data from API as an object
  // countryInfo: {},
  // };
  //  }
//   methods: {
//     async getCountryAlphaCode() {
//       //pointing with $ to the route
//       this.alpha3Code = this.$route.params.alpha3Code;
//       const res = await fetch(
//         `https://ih-countries-api.herokuapp.com/countries/${this.alpha3Code}`
//       );
//       const finalRes = await res.json();

//       this.name = finalRes.name.common;
//       this.capital = finalRes.capital[0];
//       this.area = finalRes.area;
//       this.borders = finalRes.borders;
//       this.alpha2Code = finalRes.alpha2Code;

//       // this.countryInfo = finalRes;
//     },
//   },
//   mounted() {
//     this.getCountryAlphaCode();
//   },
//   computed: {
//     countryCode() {
//       return this.$route.params.alpha3Code;
//     },
//   },

//   watch: {
//     countryCode(newCountryCode) {
//       this.getCountryAlphaCode();
//     },
//   },
 };
</script>

<style scoped>
img {
  width: 100%;
}
</style>
