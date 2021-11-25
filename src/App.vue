<template>
  <div id="app">
    <pay></pay>
  </div>
</template>

<script>
import Pay from './components/Pay.vue'
import QRCode from 'qrcodejs2'
export default {
  components: { Pay, QRCode },
  name: 'App'
}
</script>

<style>

</style>
