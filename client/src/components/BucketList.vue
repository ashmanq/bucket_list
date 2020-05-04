<template lang="html">
  <div id="favourite_countries">
    <h2>Bucket List</h2>
    <ul>
      <!-- <list-item v-for="(country, index) in bucketList" :country="country" :key="index">{{country.name}} <img class="small-flag" :src="country.flag"/> <button v-on:click="updateList">Visit!</button></list-item> -->
      <list-item v-for="(country, index) in bucketList" :country="country" :key="index"></list-item>
    </ul>
  </div>
</template>

<script>

import ListItem from '@/components/ListItem.vue';

export default {
  name: 'bucket-list',
  props: ['bucketList'],
  methods: {
    updateList(event){
      console.log(event);
      // const updatedCountry = {
      //   name: this.
      // };
    }
  },
  components: {
    'list-item': ListItem
  }
}
</script>

<style lang="css" scoped>
</style>
