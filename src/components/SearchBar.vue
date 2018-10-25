<template>
    <div class="container">
        <div class="row align-items-center">
            <div class="col">
                <input v-model="query" type="search" placeholder="Search" aria-label="Search" class="form-control">
            </div>
            <div class="col-auto">
                <button v-on:click="toggleModal()" class="btn" type="submit">BarCode</button>
            </div>
            <div id="modal" class="mx-auto pt-4">
                <e-a-n-scanner v-model="ean"/>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import EANScanner from './EANScanner'

export default {
  name: 'SearchBar',
  components: {
    EANScanner
  },
  watch: {
    query () {
      console.log(this.query)
      const url = `https://api.sallinggroup.com/v1/product-suggestions/query/${this.query}`
      axios.get(url, { headers: { Authorization: `Bearer 5d6b4a05-177f-4ca2-80e9-576b47759a85` } })
        .then(response => {
          this.$emit('input', response.data.suggestions)
        })
        .catch(err => console.log(err))
    },
    ean () {
      console.log(this.ean)
    }
  },
  data () {
    return {
      query: '',
      ean: ''
    }
  },
  methods: {
    toggleModal () {
      document.getElementById('modal').classList.toggle('d-block')
    }
  }
}
</script>

<style scoped>
    #modal {
        display: none;
        z-index: 15;
        max-width: 320px;
        max-height: 400px;
    }
</style>
