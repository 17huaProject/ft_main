<template>
  <div id="CustomListArea">
    <div class="CustomOrderBox" :key="'cl'+index" v-for="(item,index) in ListInfo">
      <!-- <div class="CO_title" v-if="item.custom_type=='COMPANY'">团建活动</div>
        <div class="CO_title" v-else>私人活动</div> -->
      <div class="CO_title">一起画Gift卡</div>
      <div class="CO_info">
        <div class="msssA" v-if="item.gift_status=='UNPAID'">未付款</div>
        <div class="msssB" v-else-if="item.gift_status=='PAID'">已付款-未送出</div>
        <div class="msssC" v-else-if="item.gift_status=='RECEIVED'">已被 {{item.receiver_nickname}} 于 {{item.received_time}} 领取</div>
        <div class="msssC" v-else>已退款</div>
        <div>数量：{{item.number}} 张</div>
        <div>创卡时间：{{item.create_time}}</div>
        <div>卡内金额：{{item.order_amount}} 元</div>
        <div>备注信息：{{item.trans_desc||'无'}}</div>
      </div>
      <div class="CO_buttonArea">
        <div style="color:#ff3f5c;border-color:#ff3f5c" v-if="item.gift_status=='UNPAID'">去支付</div>
        <div style="color:#ff3f5c;border-color:#ff3f5c" v-else-if="item.gift_status=='PAID'" @click="gotoSend(index)">去送卡</div>
        <!--<div style="color:#ff3f5c;border-color:#ffffff" v-else>已结束</div>-->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ListInfo: ''
    }
  },
  methods: {
    // retAcdTime(time) {
    //   let thisTime = new Date(time.replace(/\-/g, '/')).getTime();
    //   let todayTime = new Date(this.$store.state.SyetemBaseConfig.sys_time.replace(/\-/g, '/')).getTime();
    //   return thisTime > todayTime
    // },
    gotoSend(index) {
      let Pi = {
        order_id: this.ListInfo[index].gift_id,
        paid_amount: this.ListInfo[index].order_amount,
        pay_time: this.ListInfo[index].create_time
      }
      this.$store.commit('saveGiftCardOrderInfo', Pi)
      this.$router.push({ name: 'gcsdp', query: { g_id: this.ListInfo[index].gift_id, u_id: this.$store.state.usrInfomation.user_id } })
    },
    readList() {
      let r = {
        start_time: '',
        end_time: '',
        page_no: 1,
        page_size: 20,
        access_token: this.$store.state.usrInfomation.access_token
      }
      this.$store.commit('ConectionPara', r)
      let getString = this.$store.state.getParamString;
      let postString = getString.replace('?', '');
      this.$http.get(this.$store.state.serverHost + '/yqhbsp/gift/index' + getString).then(m => this.putInfotoList(m.data)).catch(r => this.$toast.center(r));
    },
    putInfotoList(data) {
      if (data.errcode === 200) {
        this.ListInfo = data.records
      } else {
        this.$toast.center(data.errmsg)
      }
    }
  },
  mounted() {
    this.readList()
  }
}
</script>

<style>
body {
  background-color: #f5f5f5;
}

#CustomListArea {
  width: 100%;
  float: left;
}

.CustomOrderBox {
 width: 100%;
  font-size: 14px;
  color: #666666;
  background-color: #fff;
  float: left;
  margin-bottom: 3.3%;
  
}

.CO_title {
   width: 93.4%;
  padding: 3.3%;
  float: left;
  font-size: 16px;
}

.CO_info {
  width: 100%;
  float: left;
  border-bottom: 1px solid #f5f5f5;
}

.CO_info div {
  padding: 1% 3.3%;
  margin-bottom: 1.5%;
}

.CO_buttonArea {
  width: 93.4%;
  padding: 3.3%;
  float: left;
}

.CO_buttonArea div {
  float: right;
  padding: 2px 5px;
  border: 1px solid;
  border-radius: 2px;
}

.msssA {
  font-size: 12px;
  background-color: #ff5400;
  color: #ffffff;
  padding: 2px 5px;
}

.msssB {
  font-size: 12px;
  background-color: #20B14B;
  color: #ffffff;
  padding: 2px 5px;
}

.msssC {
  font-size: 12px;
  background-color: #C29B70;
  color: #ffffff;
  padding: 2px 5px;
}
</style>
