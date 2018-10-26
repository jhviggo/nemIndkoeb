<template>
    <div class="container" style="margin-top: 10px;">
        <ul class="list-group col-auto" style="padding-right: 0; padding-left: 0;">
            <button v-if="ean || product" @click="$emit('back')" class="btn align-self-start" >Back</button>
                <li v-for="(info) in storePrices" :key="info.name" class="list-group-item row" v-bind:class="{pointer: info.name==='bilka'}" @click="redirect(info)">
                    <p style="float: left;">{{info.name}}</p>
                    <div class="col-auto" v-if="info" style="float: right;">
                        <div class="row text-right">
                            <img class="discount-image d-block" :src="discountImage" v-if="info.campaign">
                            <p>{{info.price.toFixed(2)}} kr.</p>
                        </div>
                    </div>
                </li>
            </ul>
        <img :src="productImage"/>
    </div>
</template>

<script>
import axios from 'axios'
import discountImage from '../assets/tilbud.jpg'

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
  mounted () {
  },
  data () {
    return {
      storePrices: [],
      discountImage: discountImage,
      productImage: ''
    }
  },
  methods: {
    getImageFromEAN (ean) {
      const url = 'https://api.sallinggroup.com/v1/product-images/' + ean
      axios.get(url, { headers: { Authorization: `Bearer 5d6b4a05-177f-4ca2-80e9-576b47759a85` } })
        .then(this.setImage)
        .catch(error => {
          console.log(error)
        })
    },
    setImage (response) {
      this.productImage = response.data['256']
      console.log(this.productImage)
    },
    processResponse (response) {
      const keys = Object.keys(response.data).filter(key => {
        return response.data[key]
      })

      if (response.data[keys[0]].ean) {
        this.getImageFromEAN(response.data[keys[0]].ean)
      }

      this.storePrices = keys.map(key => {
        return {
          name: key,
          price: response.data[key].price,
          campaign: response.data[key].campaign,
          ean: response.data[key].ean
        }
      }).sort((a, b) => a.price - b.price)
    },
    redirect (info) {
      if (info.name === 'bilka') {
        window.location.href = `https://beta-bilkatogo.dk/s?query=${info.ean}`
      }
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
.pointer {
    cursor: pointer;
}
</style>
