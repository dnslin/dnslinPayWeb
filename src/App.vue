<template>
  <div id="app">
    <pay></pay>
  </div>
</template>

<script>
import Pay from './components/Pay.vue'
import QRCode from 'qrcodejs2'
import ElementUI from 'element-ui'
export default {
  components: { Pay, QRCode, ElementUI },
  name: 'App'
}
</script>

<style>

</style>
