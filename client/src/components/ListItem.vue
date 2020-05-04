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

.visited {
  background-color: red;
}
</style>
