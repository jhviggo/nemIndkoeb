<template>
    <div class="container">
        <div class="row justify-content-center" style="margin-top: 10px;">
            <div class="col-8">
                <input v-model="query" type="search" placeholder="Search" aria-label="Search" class="form-control">
            </div>
            <div class="col-auto">
                <button v-on:click="toggleModal()" class="btn" >BarCode</button>
            </div>
            <div id="modal" class="mx-auto pt-4">
                <e-a-n-scanner v-if="modalShown" v-model="ean"/>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import EANScanner from './EANScanner'

export default {
  name: 'SearchBar',
  props: {
    scan: Function
  },
  components: {
    EANScanner
  },
  watch: {
    query () {
      if (this.query === '') {
        return
      } else if (this.modalStatus()) {
        this.toggleModal()
      }
      const url = `https://api.sallinggroup.com/v1/product-suggestions/query/${this.query.toLowerCase()}`
      axios.get(url, { headers: { Authorization: `Bearer 5d6b4a05-177f-4ca2-80e9-576b47759a85` } })
        .then(response => {
          this.$emit('input', response.data.suggestions)
        })
        .catch(err => console.log(err))
    },
    ean () {
      this.scan(this.ean)
    }
  },
  data () {
    return {
      query: '',
      ean: '',
      modalShown: false
    }
  },
  methods: {
    toggleModal () {
      this.modalShown = !this.modalShown
      document.getElementById('modal').classList.toggle('d-block')
      if (this.modalStatus()) {
        this.query = ''
      }
    },
    modalStatus () {
      return document.getElementById('modal').classList.contains('d-block')
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
