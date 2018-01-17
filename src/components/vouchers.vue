<template>
    <div class="mainTips">
        <div class="Tips_TopBar">
            <div @click="useAgeSide=true" :style="useAgeSide?'color:#ff3f5c;border-color:#ff3f5c':''">待使用</div>
            <div @click="useAgeSide=false" :style="!useAgeSide?'color:#ff3f5c;border-color:#ff3f5c':''">全部</div>
        </div>
        <div class="eachTips" style="flex-direction: column;" v-if="false">
            <input class="fosntSidt" placeholder="输入代金券兑换码">
            <div class="button_use">兑换</div>
        </div>
        <div class="eachTips" v-bind:key="item.id" v-if="item.status==0" v-for="(item, index) in connect" @click="useCoupon(index)">
            <div class="ticket_tips_ara_number" v-if="item.limit_amount<=getNumber||getOrderFrom">
                <div>￥
                    <span style="font-size:30px">{{item.coupon_amount}}</span>
                </div>
                <div style="font-size:14px;" v-if="item.limit_amount!=0">消费{{item.limit_amount}}可用</div>
            </div>
            <div class="ticket_tips_ara_number_dis" v-else>
                <div>￥
                    <span style="font-size:30px">{{item.coupon_amount}}</span>
                </div>
                <div style="font-size:14px;" v-if="item.limit_amount!=0">消费{{item.limit_amount}}可用</div>
            </div>
            <div class="middle_line">
                <div class="triangle-right_v"></div>
                <div class="linboder"></div>
                <div class="triangle-left_v"></div>
            </div>
            <div class="ticket_rule_area">
                <div class="ticket_rule_info">
                    <div>{{item.coupon_name}}</div>
                    <div>不与其他优惠活动同享</div>
                    <div>有效期至{{item.end_time.split(' ')[0]}}</div>
                </div>
                <div v-if="$store.state.useConpon.id==item.id" style="font-size:12px;color:#ff3f5c"><i class="el-icon-check"></i></div>
                <img v-if="item.limit_amount<=$route.query.amount&&$store.state.useConpon.id!=item.id" src="../../static/obj_img/list_icon_arrRight.png">
            </div>
        </div>
        <div class="eachTips" v-bind:key="item.id" v-if="item.status==1" v-show="!useAgeSide" v-for="(item, index) in connect">
            <div class="ticket_tips_ara_number_dis">
                <div>￥
                    <span style="font-size:30px">{{item.coupon_amount}}</span>
                </div>
                <div style="font-size:14px;">已使用</div>
            </div>
            <div class="middle_line">
                <div class="triangle-right_v"></div>
                <div class="linboder"></div>
                <div class="triangle-left_v"></div>
            </div>
            <div class="ticket_rule_area">
                <div class="ticket_rule_info">
                    <div>{{item.coupon_name}}</div>
                    <div>不与其他优惠活动同享</div>
                    <div>有效期至{{item.end_time.split(' ')[0]}}</div>
                </div>
                <img  v-if="item.limit_amount<$route.query.amount" src="../../static/obj_img/list_icon_arrRight.png">
            </div>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      getNumber: this.$route.query.amount || 0,
      getOrderFrom: this.$route.query.order || false,
      canuse: function(statue) {
        // if (this.getOrderFrom) {
        //     return 'useNum_y'
        // } else {
        //     return statue <= this.getNumber ? 'useNum_y' : 'useNum_n'
        // }
        return "useNum_y";
      },
      useAgeSide: this.$route.query.amount >= 0 ? true : false,
      connect: []
    };
  },
  created() {
    this.getCouponList();
  },
  methods: {
    getCouponList() {
      let v = {
        access_token: this.$store.state.usrInfomation.access_token
      };
      this.$store.commit("ConectionPara", v);
      let getString = this.$store.state.getParamString;
      let postString = getString.replace("?", "");
      this.$http
        .get(this.$store.state.serverHost + "/yqhbsp/coupon/index" + getString)
        .then(m => this.setCoupon(m.data))
        .catch(r => this.$toast.bottom(r));
    },
    setCoupon(data) {
      if (data.errcode === 200) {
        this.connect = data.records;
        if (data.records.length === 0) {
          this.$toast.bottom("暂无抵用券");
          this.$router.go(-1);
        }
      } else {
        this.$toast.bottom(data.errmsg);
      }
    },
    useCoupon(index) {
      if (!this.getOrderFrom) {
        if (this.connect[index].id == this.$store.state.useConpon.id) {
          this.$store.commit("saveCouponInfo", {});
          this.$router.go(-1);
          this.$message({
            message: "不使用抵用券"
          });
        } else {
          if (this.connect[index].limit_amount <= this.getNumber) {
            let c = {
              num: this.connect[index].coupon_amount,
              id: this.connect[index].id
            };
            this.$store.commit("saveCouponInfo", c);
            this.$router.go(-1);
            this.$message({
              message: "使用" + this.connect[index].coupon_amount + "元抵用券",
              type: "success"
            });
          } else {
            this.$message({
              message: "无法使用该抵用券",
              type: "error"
            });
          }
        }
        // if (this.connect[index].limit_amount <= this.getNumber) {

        // } else {
        //     this.$toast.bottom('无法使用该券')
        // }
      }
    }
  }
};
</script>

<style>
body {
  background-color: #f5f5f5;
}

.mainTips {
  display: flex;
  display: -webkit-flex;
  flex-direction: column;
  -webkit-flex-direction: column;
  width: 100%;
}

.eachTips {
  flex: 1;
  -webkit-flex: 1;
  margin: 4% 1.9% 0 1.9%;
  background-color: #ffffff;
  display: flex;
}

.Tips_TopBar {
  width: 100%;
  background-color: #fff;
  display: flex;
}

.Tips_TopBar div {
  flex: 1;
  padding: 15px 0;
  text-align: center;
  border-bottom: 2px solid #ffffff;
}

.useNum_y {
  font-size: 24px;
  font-weight: bold;
  color: #ff3f5c;
}

.useNum_n {
  font-size: 24px;
  font-weight: bold;
  color: #999999;
}

.cantUs {
  font-size: 12px;
  color: #999999;
  margin-left: 3%;
}

.middle_line {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.triangle-right_v {
  width: 0;
  height: 0;
  border-left: 8px solid transparent;
  border-top: 12px solid #f5f5f5;
  border-right: 8px solid transparent;
}

.linboder {
  width: 1px;
  border-left: 2px dashed #f5f5f5;
  flex: 1;
}

.triangle-left_v {
  width: 0;
  height: 0;
  border-left: 8px solid transparent;
  border-bottom: 12px solid #f5f5f5;
  border-right: 8px solid transparent;
}

.bottom_buttuon {
  margin-top: -6px;
  display: flex;
  display: -webkit-flex;
  padding: 5%;
  color: #666666;
  font-size: 12px;
  align-items: center;
  -webkit-align-items: center;
  justify-content: space-between;
  -webkit-justify-content: space-between;
}

.bottom_buttuon img {
  height: 18px;
}

.top_bar {
  padding: 7.5% 5% 5% 5%;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  justify-content: space-between;
  -webkit-justify-content: space-between;
  margin-bottom: -6px;
}

.left_tseTips_area {
  flex: 1;
  -webkit-flex: 1;
  font-size: 14px;
  color: #666666;
}

.left_tseTips_area div:first-child {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 4%;
  color: #333333;
}

.mj_sre_area {
  font-size: 12px;
  margin-top: 5%;
  color: #999999;
  text-align: center;
}

.right_tseTips_area {
  padding: 0 0 0 5%;
}

.right_tseTips_area div:first-child {
  font-size: 26px;
}

.ticket_tips_ara_number {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #ff3f5c;
  font-weight: bold;
  flex-direction: column;
}
.ticket_tips_ara_number_dis {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #999999;
  font-weight: bold;
  flex-direction: column;
}
.ticket_rule_area {
  display: flex;
  align-items: center;
  padding: 10px;
  flex: 1;
}
.ticket_rule_area img {
  height: 16px;
}
.ticket_rule_info {
  flex: 1;
  font-size: 12px;
  color: #999;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-content: space-between;
  height: 80px;
}
.ticket_rule_info div {
  flex: 1;
  display: flex;
  align-items: center;
  text-align: justify;
  overflow: hidden;
}
.ticket_rule_info div:first-child {
  font-size: 16px;
  color: #333;
}
.fosntSidt {
  flex: 1;
  margin: 10px 15px 2px 15px;
  border: 1px solid #f1f1f1;
  border-radius: 3px;
  padding: 0;
  height: 40px;
  line-height: 40px;
  font-size: 14px;
  outline: none;
}
.button_use {
  margin: 3px 15px 10px 15px;
  flex: 1;
  text-align: center;
  height: 40px;
  line-height: 40px;
  background-color: #ededed;
  color: #ffffff;
}
</style>
