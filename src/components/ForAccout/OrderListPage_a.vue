<template>
  <div id="main_ds">
    <div id="OrderType">
      <div :class="Select[0]" @click="aleThis(0)">全部订单</div>
      <div :class="Select[1]" @click="aleThis(1)">待付款</div>
      <div :class="Select[2]" @click="aleThis(2)">待使用</div>
      <div :class="Select[3]" @click="aleThis(3)">已退款</div>
    </div>
    <div class="OrderListArea">
      <alllist v-show="childShow[0]" :messageAer='"COMMON"'></alllist>
      <needpay v-show="childShow[1]" :messageAer='"COMMON"'></needpay>
      <canuses v-show="childShow[2]" :messageAer='"COMMON"'></canuses>
      <payouts v-show="childShow[3]" :messageAer='"COMMON"'></payouts>
    </div>
  </div>
</template>

<script>
import alllist from '../ForOrderList/AllList.vue'
import needpay from '../ForOrderList/NeedPayList.vue'
import canuses from '../ForOrderList/CanUse.vue'
import payouts from '../ForOrderList/PayOut.vue'
export default {
  data() {
    return {
      Select: ['onSelect_ol', 'diSelect', 'diSelect', 'diSelect'],
      childShow: [true, false, false, false],
    }
  },
  methods: {
    aleThis: function (num) {//切换标签
      this.Select = []; this.childShow = [];
      for (let i = 0; i < 4; i++) {
        i === num ? this.Select.push('onSelect_ol') : this.Select.push('diSelect_onSelect_ol')
        i === num ? this.childShow.push(true) : this.childShow.push(false)
      }
      document.documentElement.scrollTop = document.body.scrollTop =0;
    }
  },
  components: { alllist, needpay, canuses, payouts }
}
</script>

<style>
body {
  background-color: #f5f5f5;
}

#main_ds,
.OrderListArea {
  padding-top: 6.8%;
  float: left;
  width: 100%;
}

#OrderType {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  display: flex;
  display: -webkit-flex;
  padding: 3.3% 0;
  background-color: #ffffff;
  border-bottom: 1px solid #f5f5f5
}

#OrderType div {
  flex: 1;
  -webkit-flex: 1;
  margin: 0 2.1%;
  padding: 0 0 2.4% 0;
  text-align: center;
  font-size: 16px;
}

.onSelect_ol {
  color: #ff3f5c;
  border-bottom: 2px solid #ff3f5c;
}

.diSelect_ol {
  color: #333333;
  border-bottom: 2px solid #ffffff;
}
</style>
