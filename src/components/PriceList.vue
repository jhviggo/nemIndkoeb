<template>
    <div class="container" style="margin-top: 10px;">
        <ul class="list-group col-auto" style="padding-right: 0; padding-left: 0;">
            <button v-if="ean || product" @click="$emit('back')" class="btn align-self-start" >Back</button>
                <li v-for="(info, store ) in storePrices" :key="store" class="list-group-item">
                    <p style="float: left;">{{store}}</p>
                    <p v-if="info" style="float: right;">{{info.price}}</p>
                </li>
            </ul>
    </div>
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
      if (this.product === null) {
        this.storePrices = []
        return
      }

      const url = 'https://ideation-group5.azurewebsites.net'
      axios.get(url, { params: { articleId: this.product.id } })
        .then(this.processResponse)
        .catch(error => {
          console.log(error)
        })
    },
    ean () {
      if (this.ean === null) {
        this.storePrices = []
        return
      }
      console.log('ean' + this.ean)
      const url = 'https://ideation-group5.azurewebsites.net'
      axios.get(url, { params: { ean: this.ean } })
        .then(this.processResponse)
        .catch(error => {
          console.log(error)
        })
    }

  },
  data () {
    return {
      storePrices: []
    }
  },
  methods: {
    processResponse (response) {
      console.log(JSON.stringify(response.data))
      const keys = Object.keys(response.data).filter(key => {
        return response.data[key]
      })

      this.storePrices = keys.reduce((obj, key) => {
        obj[key] = response.data[key]
        return obj
      }
        , {})
    }
  }
}
</script>

<style scoped>

</style>
