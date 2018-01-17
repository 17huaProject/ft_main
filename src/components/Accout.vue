<template>
    <div id="stal_main">
        <img id="accoutSettingsImg"  @click="routerGo('/stp')" src="../../static/obj_img/accout_img/my_set_icon.png">
        <div id="main_title">
            <img id="accoutImg" :src="headimg">
            <div id="upsInfo_info">
                <div>{{this.$store.state.usrInfomation.nickname}}</div>
                <div>{{this.$store.state.usrInfomation.phone}}</div>
            </div>
        </div>
        <div id="cle_list">
            <!-- <div class="cle_line" @click="routerGo('/ola')">
                    <img src="../../static/obj_img/accout_img/my_order_icon.png">
                    <div>我的订单</div>
                    <img src="../../static/obj_img/accout_img/my_purse_icon.png">
                </div> -->
            <div class="order_line_title">我的订单</div>
            <div class="order_line">
                <div class="order_line_div" @click="routerGo('/ola')">
                    <img src="../../static/obj_img/accout_img/my_order_act_icon.png">
                    <div>活动订单</div>
                </div>
                <div class="order_line_div" @click="routerGo('/olp')">
                    <img src="../../static/obj_img/accout_img/my_order_product_icon.png">
                    <div>商品订单</div>
                </div>
                <div class="order_line_div" @click="routerGo('/gol')">
                    <img src="../../static/obj_img/accout_img/my_order_gift_icon.png">
                    <div>Gift卡订单</div>
                </div>
                <div class="order_line_div" @click="routerGo('/col')">
                    <img src="../../static/obj_img/accout_img/my_order_dz_icon.png">
                    <div>活动定制订单</div>
                </div>
            </div>
            <div class="cle_line" @click="routerGo('/mw')">
                <img src="../../static/obj_img/accout_img/my_wallet_icon.png">
                <div>我的钱包</div>
                <img src="../../static/obj_img/accout_img/my_purse_icon.png">
            </div>
           
           
            <div class="cle_line" @click="routerGo('/ccp')">
                <img src="../../static/obj_img/accout_img/my_collect_icon.png">
                <div>我的收藏</div>
                <img src="../../static/obj_img/accout_img/my_purse_icon.png">
            </div>
            <!-- <div class="cle_line" @click="routerGo('/cl')" style="margin-bottom:4.7%">
                <img src="../../static/obj_img/accout_img/my_linkman_icon.png">
                <div>常用信息</div>
                <img src="../../static/obj_img/accout_img/my_purse_icon.png">
            </div> -->
            <!-- <div class="cle_line" @click="routerGo('/mp')" v-show="false">
                <img src="../../static/obj_img/accout_img/my_linkman_icon.png">
                <div>更多</div>
                <img src="../../static/obj_img/accout_img/my_purse_icon.png">
            </div> -->
             <div class="cle_line" @click="routerGo('/gcb')">
                <!-- @click="$toast.center('敬请期待')" -->
                <!-- @click="routerGo('/gcb')" -->
                <img src="../../static/obj_img/accout_img/my_giftcard_icon.png">
                <div>定制Gift卡</div>
                <img src="../../static/obj_img/accout_img/my_purse_icon.png">
            </div>
             <div class="cle_line" @click="routerGo('/ct')">
                <img src="../../static/obj_img/accout_img/my_dingzhi_icon.png">
                <div>定制活动</div>
                <img src="../../static/obj_img/accout_img/my_purse_icon.png">
            </div>
             <a href="tel:02167991260">
                <div class="cle_line">
                    <img src="../../static/obj_img/more_img/more_serivice_icn1.png">
                    <div>联系客服<span>(021-67991260)</span></div>
                    <img src="../../static/obj_img/accout_img/my_purse_icon.png">
                </div>
            </a>
        </div>
        <div id="listBottom_accout" style="border-top:1px soild #eee">
            <div @click="routerGo('/list')">
                <img src="../../static/obj_img/tab_list_normal_icon.png">
                <div>活动</div>
            </div>
            <div @click="routerGo('/listCom')">
                <img src="../../static/obj_img/tab_shop_normal_icon.png">
                <div>商城</div>
            </div>
            <div>
                <img src="../../static/obj_img/tab_my_selected_icon.png">
                <div style="color:#ff3f5c">我的</div>
            </div>
            <!-- <div @click="routerGo('/mp')">
                <img src="../../static/obj_img/tab_more_normal_icon.png">
                <div>更多</div>
            </div> -->
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      headimg: this.changeImgTohttps()
      //this.$store.state.usrInfomation.avatar
    };
  },
  methods: {
    routerGo: function(paths) {
      this.$router.push({ path: paths });
    },
    changeImgTohttps() {
      if (this.$store.state.usrInfomation.avatar) {
        try {
          return this.$store.state.usrInfomation.avatar.replace(/\http:/g, "");
        } catch (error) {
          let that = this;
          setTimeout(function() {
            that.findSomeNewImgHere();
          }, 1000);
        }
      } else {
        let that = this;
        setTimeout(function() {
          that.findSomeNewImgHere();
        }, 1000);
      }
    },
    findSomeNewImgHere() {
      let a = {
        user_id: this.$store.state.usrInfomation.user_id
      };
      this.$store.commit("ConectionPara", a);
      let getString = this.$store.state.getParamString;
      let postString = getString.replace("?", "");
      this.$http
        .get(
          this.$store.state.serverHost +
            "/yqhbsp/insys/getWXUserInfo" +
            getString
        )
        .then(m => this.changeImgOfUser(m.data))
        .catch(r => console.log(r));
    },
    changeImgOfUser(data) {
      if (data.errcode == 200) {
        //SaveNewAvartarFromAPI
        this.headimg = data.record.headimgurl.replace("http:", "");
        this.$store.commit("SaveNewAvartarFromAPI", data.record.headimgurl);
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
  }
};
</script>

<style>
body {
  background-color: #f5f5f5;
}

#stal_main,
#cle_list {
  float: left;
  width: 100%;
}

#cle_list {
  margin-bottom: 70px;
}
#cle_list a {
  width: 100%;
  float: left;
}
#stal_main {
  display: flex;
  flex-direction: column;
}

#main_title {
  float: left;
  width: 93.4%;
  padding: 5.7% 3.3%;
  display: flex;
  align-items: center;
  background-color: #ff3f5c;
}

.order_line_title {
  float: left;
  width: 93.4%;
  padding: 4% 3.3%;
  background-color: #ffffff;
  border-bottom: 1px solid #f5f5f5;
}

.order_line {
  float: left;
  width: 100%;
  padding: 4% 0;
  display: flex;
  background-color: #ffffff;
  margin-bottom: 1%;
  justify-content: space-between;
}

.order_line_div {
  width: 100px;
  text-align: center;
}

.order_line_div img {
  width: 30%;
  text-align: center;
}

.order_line_div div {
  margin: 2% 0;
  font-size: 12px;
}

.cle_line {
  float: left;
  width: 93.4%;
  padding: 4% 3.3%;
  display: flex;
  background-color: #ffffff;
  margin-bottom: 1%;
}

.cle_line img {
  float: left;
  margin-top: 3px;
  height: 16px;
}

.cle_line img:first-child {
  margin-right: 3.7%;
  height: 20px;
  margin-top: 1px;
}

.cle_line div {
  flex: 1;
  -webkit-flex: 1;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #333333;
}
.cle_line div span {
  color: #999;
  font-size: 12px;
}
#accoutImg {
  width: 19.3%;
  border-radius: 100px;
  margin-right: 3.7%;
}

#upsInfo_info {
  flex: 1;
  -webkit-flex: 1;
  flex-direction: column;
}

#upsInfo_info div {
  font-size: 16px;
  color: #ffffff;
  margin-bottom: 1%;
}

#listBottom_accout {
  position: fixed;
  left: 0;
  bottom: 0;
  background-color: #ffffff;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 0 5px 1px #f1f1f1;
}

#listBottom_accout div {
  padding: 5px 5% 0 5%;
  width: 50px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

#listBottom_accout div div {
  -webkit-transform: scale(0.8);
  font-size: 12px;
}

#listBottom_accout img {
  width: 24px;
  height: 24px;
  margin-bottom: -5px;
}

#accoutSettingsImg {
  position: absolute;
  right: 0;
  top: 0;
  padding: 3%;
  width: 24px;
}
</style>
