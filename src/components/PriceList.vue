<template>
    <div class="container" style="margin-top: 10px;">
        <ul class="list-group col-auto" style="padding-right: 0; padding-left: 0;">
            <button v-if="ean || product" @click="$emit('back')" class="btn align-self-start" >Back</button>
                <li v-for="(info) in storePrices" :key="info.name" class="list-group-item">
                    <p style="float: left;">{{info.name}}</p>
                    <p v-if="info" style="float: right;"><img class="discount-image" :src="image" v-if="info.campaign">{{info.price.toFixed(2)}} kr.</p>
                </li>
            </ul>
    </div>
</template>

<script>
import axios from 'axios'
import image from '../assets/tilbud.jpg'

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
      storePrices: [],
      image: image
    }
  },
  methods: {
    processResponse (response) {
      console.log(response.data)
      const keys = Object.keys(response.data).filter(key => {
        return response.data[key]
      })

      this.storePrices = keys.map(key => {
        return {
          name: key,
          price: response.data[key].price,
          campaign: response.data[key].campaign
        }
      }).sort((a, b) => a.price - b.price)
    }
  }
}
</script>

<style scoped>
.discount-image {
    max-height: 2.5em;
    max-width: 3em;
    margin-top: -9px;
}

li {
    max-height: 4em;
    padding-top: 20px;
}
</style>
