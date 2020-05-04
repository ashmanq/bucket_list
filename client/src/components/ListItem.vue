<template lang="html">
  <div class="">
    <li>{{ country.name }} <img class="small-flag" :src="country.flag"/> <button v-on:click="updateList">Visit!</button></li>

  </div>
</template>

<script>

import {eventBus} from '@/main.js';

export default {
  name: 'list-item',
  props: ['country'],
  methods: {
    updateList(event){
      console.log(event);
      // const updatedCountry = {
      //   name: this.
      // };
    }
  },
}
</script>

<style lang="css" scoped>
</style>
