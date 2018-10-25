<template>
  <div class="home">
    <div class="row col-12">
      <SearchBar v-model="productSuggestions" :scan="scan"></SearchBar>
      <ProductList :products="productSuggestions" v-model="selectedProduct" :selectProduct="selectProduct"></ProductList>
    </div>
    <div class="row col-12">
      <PriceList :product="selectedProduct" :ean="ean"></PriceList>
    </div>
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
    },
    selectProduct(product) {
        console.log(product)
        this.selectedProduct = product;
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
