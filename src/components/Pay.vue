<template>
  <div class="qrcode" ref="qrCodeUrl"></div>
</template>

<script>
import QRCode from 'qrcodejs2'
export default {
  data: function () {
    return {
      qrcode: '123'
    }
  },
  methods: {
    creatQrCode () {
      let qrcode = new QRCode(this.$refs.qrCodeUrl, {
        text: 'https://dnslin.com',
        width: 100,
        height: 100,
        colorDark: '#000000',
        colorLight: '#ffffff',
        correctLevel: QRCode.CorrectLevel.H
      })
      this.qrcode = qrcode
      console.log(this.qrcode)
    }
  },

  mounted () {
    this.creatQrCode()
  }
}
</script>

<style>

</style>
