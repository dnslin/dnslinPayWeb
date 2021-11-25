<template>
  <div class="app">
    <!-- <div class="qrcode" ref="qrCodeUrl"></div> -->
    <div class="active">
      <el-form ref="form" :model="form" label-width="90px" >
        <span>扫码付款</span>
        <el-form-item label="">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="输入金额">
         <el-input v-model="form.region"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">创建订单</el-button>
          <el-button>取消订单</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
// import QRCode from 'qrcodejs2'
export default {
  data: function () {
    return {
      // qrcode: '',
      form: {
        name: '',
        region: ''
      }
    }
  },
  methods: {
    // creatQrCode () {
    //   let qrcode = new QRCode(this.$refs.qrCodeUrl, {
    //     text: 'https://dnslin.com',
    //     width: 100,
    //     height: 100,
    //     colorDark: '#000000',
    //     colorLight: '#ffffff',
    //     correctLevel: QRCode.CorrectLevel.H
    //   })
    //   this.qrcode = qrcode
    // },
    onSubmit () {
      console.log('submit!')
    }
  },

  mounted () {
    // this.creatQrCode()
  }
}
</script>

<style>
.app {
  text-align: center;
  background-color: rgba(202, 187, 187, 0.445);
  border-radius: 20px;
  width: 300px;
  height: 350px;
  margin: auto;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
.active{
   text-align: center;
}
.qrcode {
  display: inline-block;
  background-color: rgba(202, 187, 187, 0.445);
  border-radius: 20px;
  margin: auto;
}
img {
  width: 132px;
  height: 132px;
  background-color: #fff;
  padding: 6px;
  box-sizing: border-box;
}
body {
  background-image: url("https://i.alipayobjects.com/e/201309/19zI31wk3r_src.jpg");
  background-size: 100%;
  color: #4d4d4d;
}
</style>
