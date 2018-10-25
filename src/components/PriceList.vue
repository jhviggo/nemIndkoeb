<template>
    <ul class="list-group col-auto">
        <li v-for="(info, store )in storePrices" :key="store" class="list-group-item">{{store}} {{info.price}}</li>
    </ul>
</template>

<script>
export default {
  name: 'PriceList',
  props: {
    product: Object
  },
  watch: {
    product () {
      if (this.product === null) {
        this.storePrices = []
        return
      }
      this.storePrices = {
        'bilka': {
          'campaign': false,
          'name': 'JACOBSEN IPA 75 CL+PANT',
          'price': 37.95
        },
        'coop': {
          'campaign': false,
          'name': 'India Pale Ale 0,75 liter ekskl. pant, 6.6 % alc',
          'price': '41,75'
        },
        'foetex': {
          'campaign': false,
          'name': 'JACOBSEN IPA 75 CL+PANT',
          'price': 40.5
        },
        'netto': {
          'campaign': false,
          'name': 'JACOBSEN IPA 75 CL+PANT',
          'price': 29
        }
      }
    }
  },
  data () {
    return {
      storePrices: []
    }
  }
}
</script>

<style scoped>

</style>
