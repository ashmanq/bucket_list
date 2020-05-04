<template lang="html">
  <div id="selected_country">
    <h2> {{selectedCountry.name}} </h2>
    <img class="lrg-flag" :src="selectedCountry.flag" />

</div>
</template>

<script>
export default {
  name: 'country-detail',
  props: ['selectedCountry']
}
</script>

<style lang="css" scoped>
#selected_country {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.lrg-flag {
  height: 200px;
  border-radius: 5px;
  border-style: solid;
  border-color: lightgrey;
}

</style>
