<template>
    <div class="address phonebox">
        <mu-appbar style="width: 100%;" color="primary">
            <mu-button icon slot="left" @click="back" v-if="backshow">
                <img src="../../../static/img/back.png" />
            </mu-button>
            绑定手机号
            <!--<span v-if="isshow" style="position: absolute;right: .2rem;top: -.1rem;" @click="goEditDress">管理</span>-->
        </mu-appbar>

        <mu-list class="infoboxbut">
            <mu-list-item button :ripple="false">
                <mu-list-item-title>手机号</mu-list-item-title>
                <mu-list-item-action style="width:83%">
                    <mu-text-field v-model="phone" type='number' placeholder="请输入手机号" onkeyup="this.value=this.value.replace(/\D/g,'')" onafterpaste="this.value=this.value.replace(/\D/g,'')"></mu-text-field>
                </mu-list-item-action>
                <div v-if="!issend&&phone != ''" class="sendyzm" @click="snedyzm">发送验证码</div>
                <div v-else class="sendyzm saveboxhui">发送验证码</div>
                <div v-show="issend" class="nosendyzm">{{sec}}秒后重发</div>
            </mu-list-item>
            <mu-list-item button :ripple="false">
                <mu-list-item-title>验证码</mu-list-item-title>
                <mu-list-item-action style="width:83%">
                    <mu-text-field v-model="yzm" v-on:blur='checkNum' placeholder="验证码" type='number' onkeyup="this.value=this.value.replace(/\D/g,'')" onafterpaste="this.value=this.value.replace(/\D/g,'')"></mu-text-field>
                </mu-list-item-action>
            </mu-list-item>
            <mu-list-item button :ripple="false">
                <mu-list-item-title>邀请码</mu-list-item-title>
                <mu-list-item-action style="width:83%">
                    <mu-text-field v-model="yqm" :disabled='disyqm' placeholder="请输入邀请码"></mu-text-field>
                </mu-list-item-action>
            </mu-list-item>
        </mu-list>

        <div v-if="yqm == '' || yzm == '' || phone == ''" class="savebox saveboxhui">
            <mu-flex class="flex-wrapper" align-items="center">
                <mu-flex class="flex-demo" justify-content="center" fill>绑定</mu-flex>
            </mu-flex>
        </div>
        <div v-else class="savebox">
            <mu-flex class="flex-wrapper" align-items="center" @click='bindPone'>
                <mu-flex class="flex-demo" justify-content="center" fill>绑定</mu-flex>
            </mu-flex>
        </div>
    </div>

</template>

<script>
import {
  oauth,
  register,
  getToken,
  getTokentest,
  getUserWXInfo,
  sendPhoneYzm,
  test1,
  getShareConfig
} from "../../http/http.js";
import { getTokens, getUrlParms } from "../../common/common.js";
import { wxInit } from "../../common/share.js";
import QS from "qs";

export default {
  components: {},
  data() {
    return {
      host: this.$store.state.host,
      basehost: this.$store.state.basehost,
      openid: null,
      userid: null,
      is_phone: null,
      backshow: false,
      dTitle: "绑定手机号",
      sec: 60, //倒计时时间
      issend: false, //是否已经发送验证码
      sendtext: "发送验证码",
      phone: "", //手机号
      yzm: "", //验证码
      yqm: "", //邀请码,
      disyqm: false
    };
  },
  activated() {
    document.title = this.dTitle;
    let openid = this.$route.query.openid;
    this.openid = openid;

    let invate_code = localStorage.invate_code;
    let sharecode = this.$route.query.invate_code;
    this.is_phone = this.$route.query.is_phone;
    let userid = this.$route.query.userid;
    this.userid = userid;
    if (sharecode != "" && sharecode != undefined) {
      invate_code = sharecode;
    }
    this.yqm = "";
    this.phone = "";
    this.yam = "";
    if (invate_code) {
      this.yqm = invate_code;
      this.disyqm = true;
      this.backshow = true;
      getShareConfig({ url: window.location.href }).then(res => {
        //获取openid等
        let wx = {
          appId: res.data.appid,
          timestamp: res.data.timestamp,
          nonceStr: res.data.nonceStr,
          signature: res.data.signature
        };
        wxInit(wx, invate_code, this.$store.state.shareimg);
      });
    }
    if (openid) {
      this.setOpenid(openid);
      this.$store.commit("setOpenId", openid);
      console.log("store里面的openid");
      console.log(openid);
      this.gettoken(openid);
    }
    if (userid) {
      this.$store.commit("setUserid", userid);
      localStorage.userid = userid;
    }
  },
  mounted() {},
  watch: {
    yzm(a, b) {
      let reg = /[\u4E00-\u9FA5]|[\uFE30-\uFFA0]/gi;
      let reg1 = /^[a-zA-Z0-9]{4,6}$/;
      if (reg.test(a)) {
        this.yzm = "";
      }
      if(a.length >　6){
        this.yzm = b
      }
    }
  },
  methods: {
    checkNum(){
        let yzm = this.yzm
        if(yzm.length <=　3){
            this.$toast.error("请输入4-6位验证码");
        }else if(yzm.length >　6){
            this.yzm = b
        }
    },
    gettoken(openid) {
      getTokens(openid);
    },
    /**保存openid*/
    setOpenid(openid) {
      let obj = { name: openid };
      let str = JSON.stringify(obj);
      localStorage.obj = str;
    },
    /*绑定手机操作*/
    bindPone() {
      let phone = this.phone;
      if (phone == "") {
        this.$toast.error("请输入绑定手机号");
        return;
      }
      let telReg = !!phone.match(
        /^(0|86|17951)?(13[0-9]|15[012356789]|17[0-9]|18[0-9]|14[57])[0-9]{8}$/
      );
      if (telReg == false) {
        this.$toast.error("请输正确的手机号");
        return;
      }
      if (this.yzm == "") {
        this.$toast.error("请输入验证码");
        return;
      }
      if (this.yqm == "") {
        this.$toast.error("请输入邀请码");
        return;
      }

      this.putRegister();
    },

    /**提交用户信息*/

    putRegister() {
      let str = localStorage.obj;
      let obj = JSON.parse(str);
      let data = {
        openid: obj.name,
        code: this.yzm,
        invate_code: this.yqm,
        phone: this.phone
      };
      console.log("用户信息");
      console.log(data);
      register(QS.stringify(data)).then(res => {
        this.$store.commit("editIsBind");
        localStorage.isbind = 1; //首次注册绑定手机成功设置已经绑定手机
        let token = res.data.info.access_token;
        console.log("token");
        console.log(token);
        if (token) {
          this.$store.commit("set_token", token);
        }
        this.phone = "";
        this.yam = "";
        this.yzm = "";
        this.$router.replace("/");
        console.log("提交用户注册信息后返回的数据");
        console.log(res);
      });
    },
    /*发送验证码*/
    snedyzm() {
      let that = this;
      let tel = this.phone;
      let telReg = !!tel.match(
        /^(0|86|17951)?(13[0-9]|15[012356789]|17[0-9]|18[0-9]|14[57])[0-9]{8}$/
      );
      if (telReg == false) {
        this.$toast.error("手机号码格式不正确");
        return;
      } else {
        let timedown = setInterval(function() {
          let sec = that.sec;
          if (sec > 0) {
            that.issend = true;
            //console.log(that.sec)
            that.sec = sec - 1;
          } else {
            clearInterval(timedown);
            console.log("计时器已清");
            that.sec = 60;
            that.issend = false;
          }
        }, 1000);
        let data = QS.stringify({ phone: tel });
        sendPhoneYzm(data).then(res => {
          console.log(res);
        });
      }
    },

    back() {
      this.$router.replace(
        "/?openid=" +
          this.openid +
          "&is_phone=" +
          this.is_phone +
          "&userid=" +
          this.userid
      );
    }
  }
};
</script>

<style scoped>
@import "http://cdn.bootcss.com/material-design-icons/3.0.1/iconfont/material-icons.css";
.infoboxbut {
  background: #fff;
}
.mu-primary-color {
  background: #fff !important;
}
.mu-item-title {
  width: auto;
}

.listbox {
  width: 100%;
  margin-top: 0.15rem;
}

.infoboxbut li:not(:last-child) {
  border-bottom: 1px solid #e0e0e0;
}

.savebox {
  width: 5.05rem;
  height: 1rem;
  background: #f45f5f;
  line-height: 1rem;
  color: #fff;
  font-size: 0.32rem;
  font-weight: bold;
  margin: 1.4rem auto;
  border-radius: 0.6rem;
}
.saveboxhui {
  background: #bbb !important;
}
.sendyzm {
  position: absolute;
  right: 0.3rem;
  width: 1.53rem;
  height: 0.7rem;
  color: #fff;
  background: #f45f5f;
  font-size: 0.22rem;
  line-height: 0.7rem;
  border-radius: 0.4rem;
}
.nosendyzm {
  position: absolute;
  right: 0.3rem;
  width: 1.53rem;
  height: 0.7rem;
  background: #c3c3c3;
  font-size: 0.22rem;
  line-height: 0.7rem;
  border-radius: 0.4rem;
}
</style>