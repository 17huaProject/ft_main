<template>
    <div id="mainArea">
        <div id="detail_s" @click="routerGo('/bl')">明细</div>
        <div id="topArea">
            <!-- <div>可用余额（元）</div> -->
            <div><img src="../../static/obj_img/accout_img/my_purse_huabei_icon.png">{{Winfo.balance}}</div>
        </div>
        <div class="lineFirst" v-if="false">
            为资金安全，请立即实名认证
            <img src="../../static/obj_img/accout_img/my_purse_icon.png">
        </div>
        <div class="lineSecound" @click="toVcPage">
            <img src="../../static/obj_img/accout_img/my_purse_Coupon.png">
            <div>我的代金券</div>
            <div v-if="Winfo.coupon_num!=0">{{Winfo.coupon_num}}张</div>
            <div v-else>无</div>
            <img src="../../static/obj_img/list_icon_arrRight.png">
        </div>
         <div class="lineSecound" @click="toVcPage" v-if="false">
            <img src="../../static/obj_img/accout_img/my_giftcard_icon.png">
            <div>私人定制课时</div>
            <div>{{Winfo.coupon_num}}节</div>
            <img src="../../static/obj_img/list_icon_arrRight.png">
        </div>
         <div class="lineSecound" @click="routerGo('/sip')">
            <img src="../../static/obj_img/accout_img/my_purse_gift_icon.png">
            <div>邀请有礼</div>
            <div>有惊喜</div>
            <img src="../../static/obj_img/list_icon_arrRight.png">
        </div>
        <div class="lineSecound" @click="toGiftCard" v-if="false">
            <img src="../../static/obj_img/accout_img/my_giftcard_icon.png">
            <div>购买Gift卡</div>
            <img src="../../static/obj_img/accout_img/my_purse_icon.png">
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      Winfo: {}
    };
  },
  methods: {
    routerGo: function(paths) {
      this.$router.push({ path: paths });
    },
    toVcPage() {
      if (this.Winfo.coupon_num > 0) {
        this.$router.push({ name: "vouc", query: { order: true } });
      } else {
        this.$toast.center("你没有优惠券哦");
      }
    },
    toGiftCard() {
      this.$toast.center("敬请期待");
    },
    getUsrWalletInfo(data) {
      if (data.errcode === 200) {
        this.Winfo = data.record;
      } else {
        this.$toast.center(data.errmsg);
      }
    }
  },
  mounted() {
    let that_m = this;
    setTimeout(function() {
      that_m.$store.state.usrInfomation.access_token == ""
        ? that_m.$router.replace({ path: "/bdv" })
        : "";
    }, 1000);
  },
  created() {
    if (this.$store.state.usrInfomation.access_token=='') {
      let that = this;
      setTimeout(function() {
        let a = {
          access_token: that.$store.state.usrInfomation.access_token
        };
        that.$store.commit("ConectionPara", a);
        let getString = that.$store.state.getParamString;
        let postString = getString.replace("?", "");
        that.$http
          .get(
            that.$store.state.serverHost + "/yqhbsp/account/show" + getString
          )
          .then(m => that.getUsrWalletInfo(m.data))
          .catch(r => console.log(r));
      }, 1000);
    } else {
      let a = {
        access_token: this.$store.state.usrInfomation.access_token
      };
      this.$store.commit("ConectionPara", a);
      let getString = this.$store.state.getParamString;
      let postString = getString.replace("?", "");
      this.$http
        .get(this.$store.state.serverHost + "/yqhbsp/account/show" + getString)
        .then(m => this.getUsrWalletInfo(m.data))
        .catch(r => console.log(r));
    }
  }
};
</script>

<style>
body {
  background-color: #f5f5f5;
}

#mainArea {
  width: 100%;
}

#detail_s {
  position: absolute;
  right: 0;
  top: 0;
  padding: 3.3%;
  font-size: 16px;
  font-weight: bold;
  color: #ffffff;
}

#topArea {
  width: 93.4%;
  padding: 3.3%;
  display: flex;
  display: -webkit-flex;
  background-color: #ff3f5c;
  color: #ffffff;
  flex-direction: column;
  -webkit-flex-direction: column;
  overflow: hidden;
}

/* #topArea div:first-child {
    font-size: 16px;
} */

#topArea div {
  margin: 6.5% 0 9.3% 0;
  font-size: 30px;
  font-weight: bold;
  display: flex;
  align-items: center;
}
#topArea div img {
  height: 30px;
  margin-right: 3.3%;
}
.lineFirst {
  width: 93.4%;
  padding: 3.3%;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  justify-content: space-between;
  -webkit-justify-content: space-between;
  background-color: #fff1aa;
  color: #ff3f5c;
}

.lineFirst img {
  margin-top: 1px;
  height: 16px;
}

.lineSecound {
  width: 93.4%;
  padding: 4.7% 3.3%;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  background-color: #ffffff;
  color: #333333;
  margin-top: 1.5%;
  font-size: 14px;
}

.lineSecound img {
  margin-top: 1px;
  height: 14px;
}

.lineSecound img:first-child {
  margin-top: 1px;
  height: 20px;
  margin-right: 3.3%;
}

.lineSecound div:nth-child(2) {
  flex: 1;
  -webkit-flex: 1;
  color: #333333;
}
.lineSecound div:nth-child(3) {
  color: #999999;
  margin-right: 5px;
}
</style>
