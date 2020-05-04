<template>
  <div id="app">
    <h2>Travel Bucket List</h2>

    <country-select :countries="countries"/>

    <country-detail v-if="selectedCountry" :selectedCountry="selectedCountry">
    </country-detail>

    <!-- <button v-if="!checkExists() && selectedCountry" v-on:click="addToBucketList">Add Country</button> -->
    <button v-if="!checkExists() && selectedCountry" v-on:click="addToBucketList">Add Country</button>

    <bucket-list :bucketList="bucketList"></bucket-list>
</div>

</template>

<script>
import CountryDetail from '@/components/CountryDetail.vue';
import BucketList from '@/components/BucketList.vue';
import CountrySelect from '@/components/CountrySelect';
import BucketService from '@/services/BucketService';
import {eventBus} from '@/main.js';
export default {
  name: 'App',
  data() {
    return {
      countries: [],
      selectedCountry: null,
      bucketList: []
    }
  },
  components: {
    'country-detail': CountryDetail,
    'bucket-list': BucketList,
    'country-select': CountrySelect
  },
    mounted(){
      this.getCountries();
      this.getBucketList();

      eventBus.$on('country-selected', (country) => {
        this.selectedCountry = country;
      });

      eventBus.$on('country-updated', (updatedCountry) => {
        const index = this.bucketList.findIndex(country => country._id === updatedCountry._id);
        this.bucketList.splice(index, 1, updatedCountry);
      });
    },
    methods: {
      getCountries(){
        fetch("https://restcountries.eu/rest/v2/all")
        .then(res => res.json())
        .then(countries => this.countries = countries)
      },
      getBucketList(){
        BucketService.getBucketList()
        .then(bucketList => this.bucketList = bucketList)
      },
      addToBucketList(){
        const newCountry = {
          name: this.selectedCountry.name,
          flag: this.selectedCountry.flag,
        };
        BucketService.addCountry(newCountry)
        .then((country) => {
          this.bucketList.push(country);
        });
      },
      checkExists() {
        if (this.selectedCountry ) {
          return this.bucketList.some((country) => country.name === this.selectedCountry.name)
        } else {
          return false;
        }

      }
    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto+Condensed');

body {
  font-family: 'Roboto', sans-serif;
}

h2 {
  font-size: 2em;
  text-align: center;
}
.small-flag {
  height: 30px
}

#app {
  display: flex;
  flex-direction: column;
  align-self: center;
  align-items: center;
}



</style>
