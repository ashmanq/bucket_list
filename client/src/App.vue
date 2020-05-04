<template>
  <div id="app">
    <h1>Travel Bucket List</h1>

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

      eventBus.$on('clear-bucket-list', () => {
        BucketService.deleteAll()
        .then(() => {
          this.bucketList = [];
        })

      })
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

h1 {
  font-size: 3em;
  padding-bottom: 30px;
}
.small-flag {
  height: 30px
}

button {
  padding: 5px;
  margin:10px;
  font-size: 1.2em;
  border-radius: 5px;
}

button:hover {
  background-color: grey;
}

#app {
  display: flex;
  flex-direction: column;
  align-self: center;
  align-items: center;
}



</style>
