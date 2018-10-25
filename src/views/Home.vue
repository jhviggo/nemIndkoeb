<template>
  <div class="home">
    <div class="col-12" v-if="!selectedProduct && !ean">
      <SearchBar v-model="productSuggestions" :scan="scan"></SearchBar>
      <ProductList :products="productSuggestions" v-model="selectedProduct" :selectProduct="selectProduct"></ProductList>
    </div>
    <div class="col-12 align-content-center">
      <PriceList v-on:back="clearPrices()" :product="selectedProduct" :ean="ean"></PriceList>
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
        ean: null
      }
  },
  methods: {
      scan (ean) {
        this.ean = ean
    },
    selectProduct(product) {
        this.selectedProduct = product;
    },
    clearPrices() {
        this.scan(null)
      this.selectProduct(null)
      this.productSuggestions = [];

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
