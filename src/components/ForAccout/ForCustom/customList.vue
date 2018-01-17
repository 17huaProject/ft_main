<template>
    <div id="CustomListArea">
        <div style="text-align:center;margin:3.3% 0" v-if="ListInfo.length==0">暂无定制信息</div>
        <div class="CustomOrderBox" :key="'cl'+index" v-for="(item,index) in ListInfo">
            <div class="CO_title" v-if="item.custom_type=='COMPANY'">团建活动</div>
            <div class="CO_title" v-else>私人活动</div>
            <div class="CO_info">
                <div class="msssA" v-if="item.status==0&&retAcdTime(item.est_date)">待审核：工作人员会在24小时内和你取得联系</div>
                <div class="msssB" v-else-if="item.status==1&&retAcdTime(item.est_date)">审核通过</div>
                <div class="msssC" v-else-if="item.status==2&&retAcdTime(item.est_date)">已取消</div>
                <div class="msssC" v-else>已关闭</div>
                <div>姓名：{{item.contact}}</div>
                <div>手机号：{{item.phone}}</div>
                <div>参加人数：{{item.est_num}}人</div>
                <div>活动日期：{{item.est_date}}</div>
                <div>地址：{{item.city}}</div>
                <div>备注信息：{{item.trans_desc||'无'}}</div>
            </div>
            <div class="CO_buttonArea" v-if="retAcdTime(item.est_date)">
                <!-- <div style="color:#333333;border-color:#333333" v-if="item.status==0">取消活动</div> -->
                <div style="color:#ff3f5c;border-color:#ff3f5c" v-if="item.status==1&&item.event_status=='SOLDOUT'" @click="goActivePay(index)">查看活动</div>
                <div style="color:#ff3f5c;border-color:#ff3f5c" v-else-if="item.status==1&&item.event_status==null">活动定制中</div>
                <div style="color:#ff3f5c;border-color:#ff3f5c" v-else-if="item.status==1" @click="goActivePay(index)">去支付</div>
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
        retAcdTime(time) {
            let thisTime = new Date(time.replace(/\-/g, '/')).getTime();
            let todayTime = new Date(this.$store.state.SyetemBaseConfig.sys_time.replace(/\-/g, '/')).getTime();
            return thisTime > todayTime
        },
        readList() {
            let r = {
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionPara', r)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/custom/showMyCustom' + getString).then(m => this.putInfotoList(m.data)).catch(r => this.$toast.center(r));
        },
        putInfotoList(data) {
            if (data.errcode === 200) {
                this.ListInfo = data.records
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        goActivePay(index) {
            this.$router.push({ name: 'acinfo', query: { actinfo: this.ListInfo[index].event_id } })
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
