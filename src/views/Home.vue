<template>
  <div class="home">
    <SearchBar v-model="productSuggestions"></SearchBar>
    <ProductList :products="productSuggestions" ></ProductList>
  </div>
</template>

<script>
/* eslint-disable */
import EANScanner from '../components/EANScanner'
import SearchBar from '../components/SearchBar'
import ProductList from '../components/ProductList'

export default {
    components: {
      ProductList,
      SearchBar,
        EANScanner
    },
  data() {
      return {
        productSuggestions: []
      }
  }
}
</script>

<style>
canvas {
  position: absolute;
  top:  0;
  left: 0;
}
</style>
