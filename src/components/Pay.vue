<template>
  <div class="app">
    <div class="active">
      <form class="login" :model="table">
        <p>支付宝当面付</p>
        <input v-model="table.price" placeholder="金额" />
        <input v-model="table.email" placeholder="通知邮箱" />
        <input v-model="table.remark" placeholder="备注" />
        <input type="submit" class="btn" value="立即支付" @click="onSubmit" />
      </form>
    </div>
    <div class="dialog">
      <el-dialog
        title="付款二维码"
        :visible.sync="centerDialogVisible"
        width="24%"
        center
        custom-class="dialogClass"
        destroy-on-close
      >
        <table style="border-collapse: separate; border-spacing: 15px">
          <tr>
            <td style="text-align: center">
              <span style="font-size: 15px"
                >支付完成后会收到邮件通知,感谢您的支持</span
              >
            </td>
          </tr>
          <tr>
            <td class="tr2" style="text-align: center">
              <vue-qr
                class="qr"
                :logoSrc="imageUrl"
                :text="qrcode"
                :size="200"
              ></vue-qr>
            </td>
          </tr>
          <tr>
            <td style="text-align: center">

              <span style="font-size: 10px">扫码支付，请在<i class="el-icon-time"></i>{{result}}完成支付</span
              >
            </td>
          </tr>
          <tr>
            <td style="text-align: center">
              <img
                src="https://fb.ihouyu.cn/alipay/web/static/picture/alipay.jpg"
                alt=""
                width="130"
                height="43"
              />
            </td>
          </tr>
          <tr>
            <td style="text-align: center">
              <span style="font-size: 10px"
                >打开支付宝扫一扫，即可进行扫码支付哦</span
              >
            </td>
          </tr>
        </table>
        <span slot="footer" class="dialog-footer">
          <el-button @click="centerDialog">取 消</el-button>
          <el-button type="primary" @click="centerDialog">确 定</el-button>
        </span>
      </el-dialog>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import vueQr from 'vue-qr'
import { Message } from 'element-ui'
export default {
  data: function () {
    return {
      qrcode: '',
      table: {},
      centerDialogVisible: false,
      imageUrl: require('../assets/logo.png'),
      countdown: '300',
      result: '',
      timer: null
    }
  },
  components: {
    vueQr
  },
  methods: {
    secondToDate (result) {
      var m = Math.floor((result / 60 % 60))
      var s = Math.floor((result % 60))
      this.result = m + '分钟' + s + '秒'
    },
    // 倒计时
    sendCode () {
      this.loading() // 启动定时器
      this.timer = setInterval(() => {
        // 创建定时器
        if (this.countdown === -1) {
          this.clearTimer() // 关闭定时器
          this.skipStep()
        } else {
          this.loading()
        }
      }, 1000)
    },
    loading () {
      // 启动定时器
      let result = this.countdown-- // 定时器减1
      this.secondToDate(result)
    },
    clearTimer () {
      // 清除定时器
      clearInterval(this.timer)
      this.timer = null
      Message({
        message: '订单已超时,如未支付请重新下单',
        type: 'warning',
        duration: 5 * 1000
      })
    },
    onSubmit () {
      this.action()
    },
    centerDialog () {
      this.centerDialogVisible = false
    },
    async action () {
      let response = await axios({
        method: 'post',
        url: 'http://127.0.0.1:6789/api/pay',
        data: this.table
      })
      if (response.data.code === '202') {
        Message({
          message: '正在生成订单请稍后.......',
          type: 'success',
          duration: 5 * 1000
        })
        console.log(response.data)
        this.centerDialogVisible = true
        this.qrcode = response.data.data
        this.sendCode()
        Message({
          message: response.data.message,
          type: 'success',
          duration: 5 * 1000
        })
      }

      if (response.data.code === '201') {
        Message({
          message: '正在生成订单请稍后.......',
          type: 'success',
          duration: 5 * 1000
        })
        this.centerDialogVisible = true
        this.qrcode = response.data.data
        this.sendCode()
        Message({
          message: response.data.message,
          type: 'warning',
          duration: 5 * 1000
        })
      }
      if (response.data.code !== '201' && response.data.code !== '202') {
        Message({
          message: response.data.message,
          type: 'error',
          duration: 3 * 1000
        })
      }
    }

  }
}
</script>

<style>
.el-dialog__header {
  border-bottom: 1px solid #e8eaec80;
}
.qr {
  border-radius: 10px;
  border: 5px solid rgb(234, 95, 0);
}

.dialog {
  text-align: center;
}
.dialogClass {
   background: rgba(255, 255, 255, 0.836);
  border-radius: 10px;
}
table {
  font-family: STXihei, "微软雅黑";
}
.el-dialog .el-dialog__body {
   background: rgba(255, 255, 255, 0.021);
  display: flex;
  justify-content: center;
  align-items: center;
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
.el-dialog__header {
  border-bottom: 1px solid #e8eaec;
}
.el-dialog__footer {
  border-top: 1px solid #e8eaec;
}
.commonDialog {
  width: 100%;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.4);
  z-index: 101;
  overflow: hidden;
}
</style>
