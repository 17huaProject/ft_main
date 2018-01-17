<template>
    <div class="detail_info">
        <img class="img_Title_p" :src="cheakImgaddr(this.dpti.goods_img)">
        <!-- <div class="Tib_bar_detail" v-show="false">
                            <img src="../../static/obj_img/list_img/list_time_icon.png">
                            <div>{{eventTime}}</div>
                        </div> -->
        <div class="MainTitle_bar">
            <div class="paintion_title" style="padding-bottom:2.7%">{{dpti.name}}</div>
            <div class="charge_">
                <div class="Money_" style="font-size:18px;margin-top:5px" v-if="parseInt(dpti.count)==0">
                    已售完
                </div>
                <div class="Money_" v-else>
                    ￥{{dpti.price}}
                </div>
                <!-- <div class="Join_">
                        售出{{dpti.sale_count||0}}个
                    </div> -->
            </div>
            <!-- <div class="Experss_">
                <div>运费：10.00</div>
                <div>销量：{{dpti.sale_count||0}}件</div>
            </div> -->
        </div>
        <div class="CommitmentArea">
            <img src="../../static/obj_img/pop_right_icon.png">
            正品承诺：品质商品一起画认证
        </div>
        <div class="Content_info_actv" style="padding: 2.1% 2.9%">
            <div class="Content_title">
                <b>·</b>商品详情
                <b>·</b>
            </div>
            <div class="Content_info_box" v-html="this.unescape(dpti.content)">

            </div>
        </div>
        <div class="Introduce_Like">
            <div class="May_Like">热销商品</div>
            <div class="May_List_P">
                <areabox v-if="listArr[index].id != dpti.id" :key="'dpc'+index" v-for="(item, index) in listArr" :message='listArr[index]'></areabox>
            </div>
        </div>
        <div class="Pay_bottom" v-if="parseInt(dpti.count)!=0">
            <div class="Buy_botton" @click="CreatOrder" v-show="!Dealing">购买</div>
            <div class="Buy_botton_w" v-show="Dealing">购买</div>
            <!-- <div class="triangle-topright"></div> -->
            <div class="Charge_info">
                <div>￥{{dpti.price}}</div>
                <div>还剩{{parseInt(dpti.count)-(parseInt(dpti.sale_count)||0)}}个</div>
            </div>
        </div>
    </div>
</template>

<script>
import ActiveBox from "./ForList/ListBarStyle_5.vue";
const weekName = ["日", "一", "二", "三", "四", "五", "六"];
export default {
  data() {
    return {
      Like: true,
      Dealing: false,
      dpti: {},
      productID: {
        id: this.changeIdMsu(this.$route.query.pdcinfo) || 0,
        access_token: this.$store.state.usrInfomation.access_token || ""
      },
      listArr: [],
      joinMenber: []
    };
  },
  created() {
    window.scrollTo(0, 0);
  },
  methods: {
    LikeThisActivity(data) {
      if (data.errcode == 200 && this.likeId == "") {
        this.likeId = data.record.id;
        this.$toast.bottom("收藏成功");
      } else if (data.errcode == 200 && this.likeId != "") {
        this.likeId = "";
        this.$toast.bottom("取消收藏");
      } else {
        this.$toast.bottom(data.errmsg);
      }
    },
    cheakImgaddr(add) {
      add = this.$store.state.PROTOCOL ? add.replace(/\http:/g, "https:") : add;
      try {
        return add.indexOf("http") < 0
          ? this.$store.state.SyetemBaseConfig.image_prefix + add
          : "" + add;
      } catch (error) {
        return "";
      }
    },
    ServiceOnFree(data) {
      try {
        return this.acService.indexOf(data) >= 0;
      } catch (srror) {
        return false;
      }
    },
    changeIdMsu(data) {
      return parseInt(data);
    },
    CreatOrder() {
      this.Dealing = true;
      let comInfo = {
        OrderInfo: this.dpti
      };
      comInfo.OrderInfo.goods_img =
        this.dpti.goods_img.indexOf("http") < 0
          ? this.$store.state.SyetemBaseConfig.image_prefix +
            this.dpti.goods_img
          : "" + this.dpti.goods_img.replace(/\http:/g, "");
      this.$store.commit("setCommodityInfo", comInfo);
      this.$store.commit("saveCouponInfo", {});
      this.$store.commit("savebuyNumber", 1);
      this.$router.push({ path: "/pp" });
    },
    setJoinMenber(data) {
      if (data.errcode === 200) {
        this.joinMenber = data.records;
      } else {
        this.$toast.center(data.errmsg);
      }
    },
    getDetailMaster() {
      this.$store.commit("ConectionPara", this.productID);
      let getString = this.$store.state.getParamString;
      let postString = getString.replace("?", "");
      this.$http
        .get(this.$store.state.serverHost + "/yqhbsp/goods/show" + getString)
        .then(m => this.setProductDetail(m.data))
        .catch(r => console.log(r));
    },
    setProductDetail(data) {
      if (data.errcode === 200) {
        this.dpti = data.record;
      } else {
        this.$toast.center(data.errmsg);
      }
    },
    unescape(html) {
      html = this.$store.state.PROTOCOL
        ? html.replace(/\http:/g, "https:")
        : html;
      try {
        return html
          .replace(html ? /&(?!#?\w+;)/g : /&/g, "&amp;")
          .replace(/&lt;/g, "<")
          .replace(/&gt;/g, ">")
          .replace(/&quot;/g, '"')
          .replace(/&#39;/g, "'")
          .replace(/&mdash;/g, "-")
          .replace(/&nbsp;/g, " ")
          .replace(/&ldquo;/g, "“")
          .replace(/&rdquo;/g, "”");
      } catch (error) {}
    },
    getListProduct() {
      let ListConfig = {
        page_no: 1,
        page_size: 5
      };
      this.$store.commit("ConectionPara", ListConfig);
      let getString = this.$store.state.getParamString;
      let postString = getString.replace("?", "");
      this.$http
        .get(this.$store.state.serverHost + "/yqhbsp/goods/index" + getString)
        .then(m => this.setProduct(m.data))
        .catch(r => console.log(r));
    },
    setProduct(data) {
      this.listArr = data.records;
    }
  },
  watch: {},
  components: {
    areabox: ActiveBox
  },
  mounted() {
    this.getDetailMaster();
    this.getListProduct();
    // let share = {
    //   sharetitle: "一起画专用：" + this.dpti.name,
    //   sharelink:
    //     "https://wx.17hua.me/#/dp?pdcinfo=" + this.$route.query.pdcinfo,
    //   shareimgUrl:
    //     this.dpti.goods_img.indexOf("http") < 0
    //       ? this.$store.state.SyetemBaseConfig.image_prefix +
    //         this.dpti.goods_img
    //       : "" + this.dpti.goods_img.replace(/\http:/g, ""),
    //   shareDesc: "一起画，就购了！"
    // };
    // this.$store.commit("useShareFun", share);
    // if (sessionStorage.getItem('CreateOrder')) {
    //     sessionStorage.removeItem('CreateOrder')
    //     this.CreatOrder();
    // }
  }
};
</script>

<style>
body {
  background-color: #f5f5f5;
}

.Pay_bottom {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 60px;
}

.detail_info,
.img_Title_p,
.charge_,
.Free_sent,
.Introduce_Like {
  float: left;
  width: 100%;
}

.Introduce_Like {
  padding-bottom: 60px;
}

.img_Title_p,
.charge_,
.MainTitle_bar,
.Free_sent,
.Active_address_,
.Member_Join,
.Content_info_actv {
  background-color: #ffffff;
}

.charge_,
.Free_sent,
.Free_Pade,
.Active_address_,
.Member_Join {
  display: flex;
  display: -webkit-flex;
}

.MainTitle_bar {
  float: left;
  width: 94.2%;
  padding: 0 2.9% 2.1% 2.9%;
  margin-bottom: 1.4%;
}

.Member_Join {
  float: left;
  width: 94.2%;
  padding: 2.1% 2.9%;
  margin-bottom: 1.4%;
}

.Active_address_ {
  float: left;
  width: 94.2%;
  padding: 0 2.9%;
  margin-bottom: 1%;
}

.Free_sent {
  float: left;
  width: 94.2%;
  padding: 2.1% 2.9%;
  margin-bottom: 1%;
}

.Content_info_actv {
  float: left;
  width: 94.2%;
  padding: 2.1% 2.9% 50px 2.9%;
  border-bottom: 1px solid #f2f2f2;
}

.Tib_bar_detail {
  float: left;
  width: 92%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  font-size: 12px;
  color: #ffffff;
  height: 40px;
  margin-top: -40px;
  padding: 0 4%;
}

.Tib_bar_detail img {
  width: 3.3%;
  margin-right: 1.1%;
}

.Tib_bar_detail div {
  margin-top: -1px;
  margin-right: 3.2%;
}

/*标题栏*/

.Like_cover_detail {
  position: absolute;
  right: 0;
  top: 0;
  width: 7.2%;
  margin: 3.8% 3.2%;
}

.paintion_title {
  font-weight: bold;
  padding-top: 2.7%;
  font-size: 18px;
  color: #333333;
}

.level_Area {
  display: flex;
  display: -webkit-flex;
  color: #ff3300;
  font-weight: bold;
  line-height: 15px;
  padding-top: 3%;
  font-size: 14px;
  align-items: flex-end;
  -webkit-align-items: flex-end;
}

.level_Area div {
  display: flex;
  display: -webkit-flex;
  color: #333333;
  font-weight: normal;
  line-height: 12px;
  margin-right: 5px;
}

.level_Area img {
  width: 16px;
  height: 16px;
  margin: -1px 4px 0 0;
}

.end_anonce {
  width: 100%;
  text-align: right;
  font-size: 12px;
  margin-top: -5px;
  color: #666666;
}

.Money_ {
  font-size: 24px;
  color: #ff3f5c;
  font-weight: bold;
}

.Experss_ {
  margin-top: 2.7%;
  padding-top: 2.7%;
  width: 100%;
  height: 20px;
  line-height: 20px;
  float: left;
  border-top: 1px solid #f5f5f5;
  display: flex;
}
.Experss_ div {
  flex: 1;
  font-size: 12px;
  color: #333;
}
.Join_ {
  font-size: 12px;
  color: #666666;
  margin-left: 2%;
  line-height: 38px;
}

.count_down_box {
  display: flex;
  display: -webkit-flex;
  flex: 1;
  -webkit-flex: 1;
  justify-content: flex-end;
  -webkit-justify-content: flex-end;
  line-height: 38px;
  font-size: 14px;
  color: #666666;
  align-items: center;
  -webkit-align-items: center;
}

.count_down_box div {
  width: 24px;
  height: 24px;
  line-height: 24px;
  text-align: center;
  background-color: #ff3f5c;
  -webkit-border-radius: 5px;
  border-radius: 5px;
  margin: 0 5px;
  font-size: 14px;
  color: #ffffff;
}

.count_down_box span {
  height: 24px;
  line-height: 24px;
  text-align: center;
  -webkit-border-radius: 5px;
  border-radius: 5px;
  margin: 0 0 0 5px;
  font-size: 14px;
  color: #ff3f5c;
}

.CommitmentArea {
  display: flex;
  float: left;
  width: 94.2%;
  padding: 0.7% 2.9% 2.1% 2.9%;
  align-items: center;
  font-size: 12px;
  color: #666;
}
.CommitmentArea img {
  height: 10px;
  margin-right: 5px;
}

/*免费服务*/

.Free_Pade {
  padding: 0 8% 0 0;
  float: left;
  display: flex;
  display: -webkit-flex;
  font-size: 12px;
  line-height: 14px;
  justify-content: center;
  -webkit-justify-content: center;
  color: #666666;
}

.Free_Pade img {
  height: 14px;
  margin-right: 2px;
}

/*举办地*/

.Active_address_ {
  /* font-weight: bold; */
  color: #333333;
  align-items: center;
  -webkit-align-items: center;
  justify-content: space-between;
  -webkit-justify-content: space-between;
}

.Active_address_ div {
  padding: 2.9% 0;
  font-size: 14px;
  float: left;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
}

.Active_address_ img {
  float: right;
  height: 14px;
  margin: 0 0 0 5px;
}

/*参加朋友*/

.Member_Join {
  flex-direction: column;
  -webkit-flex-direction: column;
}

.jo_text {
  float: left;
  flex: 1;
  -webkit-flex: 1;
  font-size: 14px;
  color: #333333;
}

.jo_img {
  margin: 2.7% 0;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  justify-content: space-between;
  -webkit-justify-content: space-between;
}

.jo_img img {
  -webkit-border-radius: 100px;
  border-radius: 100px;
  width: 16%;
}

/*活动内容*/

.Content_title {
  display: flex;
  display: -webkit-flex;
  justify-content: center;
  -webkit-justify-content: center;
  align-items: center;
  -webkit-align-items: center;
  flex: 1;
  -webkit-flex: 1;
  font-weight: bold;
  font-size: 18px;
  padding: 3.3% 0 2.7% 0;
}

.Content_title b {
  font-size: 24px;
  margin: 0 10px;
}

.Content_info_box {
  float: left;
  width: 100%;
  /* text-indent: 2em;
    line-height: 1.5em;
    letter-spacing: 2px;
    word-spacing: 4px; */
  color: #666666;
  overflow-x: hidden;
}

.Content_info_box div,
.Content_info_box p {
  float: left;
  width: 100%;
  max-width: 100%;
  height: auto;
}

.Content_info_box img {
  float: left;
  width: 100%;
  max-width: 100%;
  height: auto;
  padding: 3.3% 0 2.7% 0;
}

.Content_info_box_P {
  float: left;
  width: 100%;
  overflow-x: hidden;
  display: flex;
  display: -webkit-flex;
  justify-content: center;
  -webkit-justify-content: center;
  align-items: center;
  -webkit-align-items: center;
}

.Content_info_box_P img {
  width: 120px;
  height: 120px;
  margin: 0 3.3% 0 0;
  float: left;
  -webkit-border-radius: 60px;
  border-radius: 60px;
}

.Content_Painter_right {
  flex: 1;
  -webkit-flex: 1;
  display: flex;
  display: -webkit-flex;
  flex-direction: column;
  -webkit-flex-direction: column;
}

.c_text_panter {
  flex: 1;
  -webkit-flex: 1;
  text-indent: 2em;
  line-height: 1.5em;
  letter-spacing: 2px;
  word-spacing: 4px;
  font-size: 14px;
  color: #666666;
}

.level_panter {
  margin-top: 10px;
  width: 100%;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  font-size: 14px;
  color: #666666;
}

.level_panter img {
  float: left;
  width: 14px;
  height: 14px;
  margin: 0 5px 0 0;
}

.comPass_painter {
  margin-top: 10px;
  width: 100%;
  display: flex;
  display: -webkit-flex;
  flex-direction: column;
  -webkit-flex-direction: column;
  font-size: 12px;
  color: #666666;
}

.comPass_painter div {
  float: left;
  padding-bottom: 5px;
}

.comPass_painter img {
  float: left;
  width: 14px;
  height: 14px;
}

/*列表内容*/

.May_Like {
  width: 100%;
  text-align: center;
  font-weight: bold;
  font-size: 18px;
  margin: 3.3% 0 0 0;
}

.May_list {
  width: 100%;
  float: left;
}

/*购买按钮部分*/

.Buy_botton {
  position: absolute;
  right: 0;
  bottom: 0;
  width: 67.6%;
  text-align: center;
  background-color: #ff3f5c;
  color: #ffffff;
  font-size: 18px;
  height: 18px;
  line-height: 18px;
  font-weight: bold;
  padding: 16px 0;
  z-index: 10;
}

.Buy_botton_w {
  position: absolute;
  right: 0;
  bottom: 0;
  width: 67.6%;
  text-align: center;
  background-color: #999999;
  color: #ffffff;
  font-size: 18px;
  height: 18px;
  line-height: 18px;
  font-weight: bold;
  padding: 16px 0;
  z-index: 10;
}

.triangle-topright {
  width: 0;
  height: 0;
  border-top: 41px solid rgba(200, 0, 0, 0.5);
  border-left: 4px solid transparent;
  position: absolute;
  right: 0;
  bottom: 0;
  margin-right: 67.6%;
  z-index: 9;
}

.Charge_info {
  position: absolute;
  left: 0;
  bottom: 0;
  margin-right: 67.6%;
  width: 33.4%;
  background-color: rgba(255, 255, 255, 0.9);
  height: 50px;
  z-index: 8;
  display: flex;
  display: -webkit-flex;
  justify-content: center;
  -webkit-justify-content: center;
  flex-direction: column;
  -webkit-flex-direction: column;
}

.Charge_info div:first-child {
  text-align: center;
  font-size: 18px;
  color: #ff3f5c;
  font-weight: bold;
  line-height: 18px;
}

.Charge_info div:last-child {
  text-align: center;
  font-size: 12px;
  color: #999999;
  line-height: 14px;
}

.findDetailThis {
  width: 94.2%;
  padding: 0 2.9%;
  height: 40px;
  line-height: 40px;
  margin-top: -40px;
  float: left;
  text-align: right;
  font-size: 14px;
  color: #ff3f5c;
  text-decoration: underline;
}

.May_List_P {
  width: 100%;
  margin-bottom: 60px;
}

.Content_venue_img {
  width: 100%;
  margin-bottom: 5%;
}
</style>
