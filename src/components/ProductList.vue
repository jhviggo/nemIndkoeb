<template>
    <ul class="list-group col-auto">
        <li v-on:click="selectProduct(product)" v-for="product in products" :key="product.id" class="list-group-item">{{product.title}}</li>
    </ul>
</template>

<script>
export default {
  name: 'ProductList',
  props: {
    products: Array,
    selectProduct: Function
  }
}
</script>
