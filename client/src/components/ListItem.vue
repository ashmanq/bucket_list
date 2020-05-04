<template lang="html">
  <div class="">
    <li v-bind:class="countryVisited()">{{ country.name }} <img class="small-flag" :src="country.flag"/>
      <button v-if="!country.visited" v-on:click="updateList">Visit!</button></li>

  </div>
</template>

<script>
import BucketService from '@/services/BucketService.js'
import {eventBus} from '@/main.js';

export default {
  name: 'list-item',
  props: ['country'],
  methods: {
    updateList(event){
      const updatedCountry = {
        name: this.country.name,
        flag: this.country.flag,
        visited: true
      };

      BucketService.updateCountry(this.country._id, updatedCountry)
      .then((country) => eventBus.$emit('country-updated', country));
    },

    countryVisited() {
      if (this.country.visited){
        return 'visited';
      }
    }
  },
}
</script>

<style lang="css" scoped>


li {
  font-size: 1.2em;
  padding:10px;
  margin: 10px;
  list-style: none;
  margin-left: 0;
  border-radius: 5px;
  border-style: solid;
  border-color: black;
  border-width: 2px;
  text-align: center;
  justify-content: center;
}
.visited {
  background-color: green;

}

button {
  padding: 2px;
  margin:10px;
  font-size: 1.2em;
}

button:hover {
  background-color: grey;
}




</style>
