<template>
  <div>
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
  </div>
  <!-- using an object to store all data properties and use it on template -->
 
</template>

<script>
export default {
  name: "CountriesDetails",
  data() {
    return {
      name: "",
      capital: "",
      alpha3Code: "",
      area: "",
      borders: [],
      alpha2Code: "",
      // passing data from API as an object
      // countryInfo: {},
    };
  },
  methods: {
    async getCountryAlphaCode() {
      //pointing with $ to the route
      this.alpha3Code = this.$route.params.alpha3Code;
      const res = await fetch(
        `https://ih-countries-api.herokuapp.com/countries/${this.alpha3Code}`
      );
      const finalRes = await res.json();

      this.name = finalRes.name.common;
      this.capital = finalRes.capital[0];
      this.area = finalRes.area;
      this.borders = finalRes.borders;
      this.alpha2Code = finalRes.alpha2Code;

      // this.countryInfo = finalRes;
    },
  },
  mounted() {
    this.getCountryAlphaCode();
  },
  computed: {
    countryCode() {
      return this.$route.params.alpha3Code;
    },
  },

  watch: {
    countryCode(newCountryCode) {
      this.getCountryAlphaCode();
    },
  },
};
</script>

<style scoped>
img {
  width: 100%;
}
</style>
