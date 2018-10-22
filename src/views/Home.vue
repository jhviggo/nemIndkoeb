<template>
  <div class="home">
    <div id="item">i am a thing</div>
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'
import Quagga from 'quagga' // ES6

/* eslint-disable */
export default {
  name: 'home',
  components: {
    HelloWorld
  },
  mounted() {
    Quagga.init({
      inputStream : {
        name : "Live",
        type : "LiveStream",
        target: document.querySelector('#item')    // Or '#yourElement' (optional)
      },
      decoder : {
        readers : ["code_128_reader"]
      }
  }, function(err) {
      if (err) {
        console.log(err);
        return
      }
      console.log("Initialization finished. Ready to start");
      Quagga.start();
    });
  }
}
</script>

<style>
.home {
  width: 100px;
  height: 100px;
  background-color: lightgray;
}
</style>
