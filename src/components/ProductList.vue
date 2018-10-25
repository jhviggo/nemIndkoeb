<template>
    <ul class="list-group col-auto" style="margin-top: 10px; padding-right:0;">
        <li v-on:click="$emit('input', product)" v-for="product in products" :key="product.id" class="list-group-item list-group-item-action" style="text-align: left;">{{product.title}}</li>
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
