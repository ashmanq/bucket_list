<template lang="html">
  <div id="favourite_countries">
    <h2>Bucket List</h2>
    <button v-if="bucketList.length" v-on:click="clearlist">Clear List</button>
    <list-item v-for="(country, index) in bucketList" :country="country" :key="index"></list-item>
  </div>
</template>

<script>

import ListItem from '@/components/ListItem.vue';
import {eventBus} from '@/main.js';

export default {
  name: 'bucket-list',
  props: ['bucketList'],
  mounted(){

  },
  methods: {
    clearlist() {
      eventBus.$emit('clear-bucket-list');
    }
  },
  components: {
    'list-item': ListItem
  }
}
</script>

<style lang="css" scoped>
 #favourite_countries {
   display: flex;
   flex-direction: column;
 }
</style>
