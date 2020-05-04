<template lang="html">
  <div v-bind:class="countryVisited()" class="container">
    <p >{{ country.name }}</p>
    <img class="small-flag" :src="country.flag"/>
    <button v-if="!country.visited" v-on:click="updateList">Visit!</button>

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

.container {
  display: flex;
  align-content: center;
  justify-content: center;
  border-radius: 5px;
  border-style: solid;
  border-color: black;
  border-width: 2px;
  padding:5px;
  margin: 10px;
}

p {
  font-size: 1.2em;
  width: 100%;
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
  border-radius: 5px;
}

button:hover {
  background-color: grey;
}

img{
  height:60px;
  width: 90px;

}


</style>
