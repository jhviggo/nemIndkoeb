<template>
  <div class="home">
    <SearchBar v-model="productSuggestions" :scan="scan"></SearchBar>
    <ProductList :products="productSuggestions" v-model="selectedProduct"></ProductList>
    <PriceList :="selectedProduct" :ean="ean"></PriceList>
  </div>
</template>

<script>
/* eslint-disable */
import EANScanner from '../components/EANScanner'
import SearchBar from '../components/SearchBar'
import ProductList from '../components/ProductList'
import PriceList from '../components/PriceList'

export default {
    components: {
      PriceList,
      ProductList,
      SearchBar,
        EANScanner
    },
  data() {
      return {
        productSuggestions: [],
        selectedProduct: null,
        ean: ''
      }
  },
  methods: {
      scan (ean) {
        console.log("ean: " + ean)
        this.ean = ean
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
