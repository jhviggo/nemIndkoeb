<template>
  <div class="home">
    <div id="modal" class="mx-auto">
      <e-a-n-scanner/>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import EANScanner from '../components/EANScanner'

export default {
    components: {
        EANScanner
    }
}
</script>

<style>
#modal {
  display: none;
  position: absolute;
  z-index: 15;
  max-width: 320px;
  max-height: 400px;
}

canvas {
  position: absolute;
  top:  0;
  left: 0;
}
</style>
