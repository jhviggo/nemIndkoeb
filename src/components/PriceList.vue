<template>
    <ul class="list-group col-auto">
        <li v-for="(info, store ) in storePrices" :key="store" class="list-group-item">{{store}} {{info.price}}</li>
    </ul>
</template>

<script>
import axios from 'axios'

export default {
  name: 'PriceList',
  props: {
    product: Object,
    ean: String
  },
  watch: {
    product () {
      const url = 'https://ideation-group5.azurewebsites.net'
      axios.get(url, { params: { articleId: this.product.id } })
        .then(response => {
          console.log(response.data)
          this.storePrices = response.data
        })
        .catch(error => {
          console.log(error)
        })
    },
    ean () {
      console.log('ean' + this.ean)
      const url = 'https://ideation-group5.azurewebsites.net'
      axios.get(url, { params: { ean: this.ean } })
        .then(response => {
          console.log(response.data)
          this.storePrices = response.data
        })
        .catch(error => {
          console.log(error)
        })
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
