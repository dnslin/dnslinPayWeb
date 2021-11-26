<template>
  <div class="app" >
    <div class="active" >
      <form class="login" :model="table">
        <p>支付宝当面付</p>
        <input v-model="table.price" placeholder="金额" />
        <input v-model="table.email" placeholder="通知邮箱" />
        <input v-model="table.remark" placeholder="备注" />
        <input type="submit" class="btn" value="立即支付" @click="onSubmit"/>
      </form>
    </div>
     <div class="qrcode" ref="qrCodeUrl"></div>
  </div>
</template>

<script>
import QRCode from 'qrcodejs2'
import axios from 'axios'
export default {
  data: function () {
    return {
      qrcode: null,
      table: {
      }
    }
  },
  methods: {
    creatQrCode (text) {
      let qrcode = new QRCode(this.$refs.qrCodeUrl, {
        text: text,
        width: 100,
        height: 100,
        colorDark: '#000000',
        colorLight: '#ffffff',
        correctLevel: QRCode.CorrectLevel.H
      })
      console.log(this.qrcode)
      this.qrcode = qrcode
    },
    onSubmit () {
      this.action()
    },
    async action () {
      let response = await axios({
        method: 'post',
        url: 'http://127.0.0.1:6789/api/pay',
        data: this.table
      })
      if (response.data.code === '200') {
        console.log(response.data.data)
        let text = response.data.data
        this.creatQrCode(text)
      } else {}
    }
  }

}
</script>

<style>
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
  background-size: cover;
  background-attachment: fixed;
}
* {
  user-select: none;
  /* 无法选中，整体感更强 */
}
.login {
  position: absolute;
  top: 50%;
  margin-top: -200px;
  left: 50%;
  margin-left: -200px;
  /* absolute居中的一种方法 */
  background-color: rgba(117, 182, 223, 0.315);
  width: 400px;
  height: 440px;
  border-radius: 25px;
  text-align: center;
  padding: 5px 40px;
  box-sizing: border-box;
  /* 这样padding就不会影响大小 */
}

p {
  font-size: 40px;
  font-weight: 600;
}
input {
  background-color: rgba(255, 253, 253, 0);
  width: 100%;
  height: 48px;
  margin-bottom: 10px;
  border: none;
  border-bottom: 2px solid rgba(192, 185, 185, 0.329);
  /* 下面的会覆盖上面的步伐 */
  outline: none;
  font-size: 22px;
}

.btn {
  background-color: #53bba4d2;
  width: 38%;
  height: 48px;
  border-radius: 8px;
  margin-top: 40px;
  font-size: 28px;
  font-weight: 600;
  color: white;
}
.btn:hover {
  background-color: #3f977b;
}
</style>
