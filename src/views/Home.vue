<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <search-drinks></search-drinks>
  </div>
</template>

<script>
// @ is an alias to /src
import SearchDrinks from '@/components/SearchDrinks.vue'
export default {
  name: 'Home',
  components: {
    SearchDrinks
  }
}
</script>
