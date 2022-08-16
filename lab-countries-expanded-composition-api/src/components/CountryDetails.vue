<template>
  <div class="container d-flex flex-column justify-center">
    <!-- <img
      style="width: 300px"
      :src="`https://flagcdn.com/w320/${alpha2Code.toLowerCase()}.png`"
      alt=""
    /> -->
    <ul class="list-group list-group-flush">
      <li class="list-group-item">
        <h3>Name:</h3>
        <p>{{ name }}</p>
      </li>
      <li class="list-group-item">
        <h3>Capital:</h3>
        <p>{{ capital }}</p>
      </li>
      <li class="list-group-item">
        <h3>Area:</h3>
        <p>{{ area }}</p>
      </li>
      <li class="list-group-item">
        <h3>Borders:</h3>
        <p v-if="borders.length === 0">
          This country has no borders<br />
          Maybe is island...
        </p>
        <p v-else v-for="(border, index) in borders" :key="index">
          <router-link :to="`/list/${border}`">{{ border }}</router-link>
        </p>
      </li>
    </ul>

    <!-- Using an Object -->
    <!-- <ul class="list-group list-group-flush">
      <li class="list-group-item">
        <h3>Name:</h3>
        <p>{{ countryInfo.name.common }}</p>
      </li>
      <li class="list-group-item">
        <h3>Capital:</h3>
        <p>{{ countryInfo.capital[0] }}</p>
      </li>
      <li class="list-group-item">
        <h3>Area:</h3>
        <p>{{ countryInfo.area }}</p>
      </li>
      <li class="list-group-item">
        <h3>Borders:</h3>
        <p v-if="countryInfo.borders.length === 0">
          This country has no borders<br />
          Maybe is island...
        </p>
        <p v-else v-for="border in countryInfo.borders">
          <router-link :to="`/list/${border}`">{{ border }}</router-link>
        </p>
      </li>
    </ul> -->
  </div>
</template>

<script>
import { ref, computed, onMounted, watch } from "vue";
import { useRoute, useRouter } from "vue-router";
export default {
  setup() {
    const name = ref("");
    const capital = ref("");
    const alphacode = ref("");
    const area = ref("");
    const borders = ref([]);
    const alpha2Code = ref("");
    const countryInfo = ref({});

    // const countryData = reactive({
    //   name: "",
    //   capital: "",
    //   alphacode: "",
    //   area: "",
    //   borders: [],
    //   alpha2Code: "",
    //   countryInfo: {},
    // });

    const route = useRoute();

    //THis is how we import methods
    const getCountryByAlphaCode = async () => {
      // console.log($route.params);
      //console.log($route.params.alpha3Code);
      // const alphacode = route.params.alpha3Code;
      const alphacode = route.params.alpha3Code;
      const response = await fetch(
        `https://ih-countries-api.herokuapp.com/countries/${alphacode}`
      );
      const finalResponse = await response.json();
      console.log(finalResponse);

      name.value = finalResponse.name.common;
      capital.value = finalResponse.capital[0];
      area.value = finalResponse.area;
      borders.value = finalResponse.borders;
      alpha2Code.value = finalResponse.alpha2Code;

      // send as an array
      const countryInfo = ref(finalResponse);
      return { name, capital, area, borders, alpha2Code, countryInfo };
    };

    // boilerPlate to pass computed properties
    // computed(function () { });

    const countryCode = computed(function () {
      return route.params.alpha3Code;
    });

    watch(countryCode, (newCountryCode) => {
      getCountryByAlphaCode();
    });

    onMounted(() => {
      getCountryByAlphaCode();
    });

    return {
      name,
      capital,
      alphacode,
      area,
      borders,
      alpha2Code,
      countryInfo,
      getCountryByAlphaCode,
      countryCode,
    };
  },

  // data() {
  //   return {
  //     name: "",
  //     capital: "",
  //     alphacode: "",
  //     area: "",
  //     borders: [],
  //     alpha2Code: "",

  //     countryInfo: {},
  //   };
  // },
  // methods: {
  //   async getCountryByAlphaCode() {
  //     // console.log(this.$route);
  //     // console.log(this.$route.params);
  //     // console.log(this.$route.params.alpha3Code);
  //     this.alphacode = this.$route.params.alpha3Code;
  //     const response = await fetch(
  //       `https://ih-countries-api.herokuapp.com/countries/${this.alphacode}`
  //     );
  //     const finalResponse = await response.json();
  //     console.log(finalResponse);

  //     this.name = finalResponse.name.common;
  //     this.capital = finalResponse.capital[0];
  //     this.area = finalResponse.area;
  //     this.borders = finalResponse.borders;
  //     this.alpha2Code = finalResponse.alpha2Code;

  //     // send as an array
  //     this.countryInfo = finalResponse;
  //   },
  // },
  // mounted() {
  //   this.getCountryByAlphaCode();
  // },
  // computed: {
  //   countryCode() {
  //     return this.$route.params.alpha3Code;
  //   },
  // },
};
</script>

<style></style>

<!-- 
:src="`https://flagpedia.net/data/flags/icon/72x54/${alpha2Code.toLowerCase()}.png`" -->
