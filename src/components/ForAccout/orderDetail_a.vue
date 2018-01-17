<template>
    <div id="orderDetail">
        <div id="recomTime" v-show="orderinfo.order_status=='UNPAID'">剩余支付时间 {{rocallTime}}</div>
        <div class="jinDds" v-if="orderinfo.order_status=='UNPAID'">
            <div>订单编号：{{orderinfo.order_id}}</div>
            <div>创建时间：{{orderinfo.create_time}}</div>
        </div>
        <div id="banTop">
            <div class="ac_name">{{orderinfo.order_name}}</div>
            <div class="ac_info">
                <div class="tips_img_onImg" v-show="orderinfo.order_status=='SUCCESS'" style="background-color:#ff3f5c">可使用</div>
                <div class="tips_img_onImg" v-show="orderinfo.order_status=='UNPAID'" style="background-color:#FDA649">未支付</div>
                <div class="tips_img_onImg" v-show="orderinfo.order_status=='FINISH'" style="background-color:#C29B70">已使用</div>
                <div class="tips_img_onImg" v-show="orderinfo.order_status=='REFUND'" style="background-color:#C29B70">已退款</div>
                <div class="tips_img_onImg" v-show="orderinfo.order_status=='CLOSED'" style="background-color:#C29B70">已关闭</div>
                <!-- <img :src="this.imgTitle"> -->
                <div class="ac_info_img" :style="{background: 'url(\''+this.imgTitle+'\')'+ 'center center no-repeat',backgroundSize:'auto 100%'}"></div>
                <div class="ac_info_text">
                    <div>时间：{{getTimeSetForOL(orderinfo.event_time)}}</div>
                    <div>举办地：{{orderinfo.venue_name}}</div>
                    <div>{{orderinfo.number}}张票 | 总价：￥{{orderinfo.order_amount}}</div>
                </div>
            </div>
            <div class="ac_ad_info">
                <!-- <div>{{orderinfo.venue_name}}</div> -->
                <div @click="jumpToamap">地址：{{orderinfo.address}}
                    <img src="../../../static/obj_img/list_icon_arrRight.png">
                </div>
            </div>
        </div>
        <div id="banMiddle" v-if="orderinfo.order_status=='SUCCESS'||orderinfo.order_status=='FINISH'">
            <div class="bt_text">验证码区</div>
            <div class="ticket_box" :key="'tici'+index" v-for="(items,index) in orderinfo.tickets">
                <div class="ticket_ta" v-if="items.used_flag==0">
                    可使用
                    <span>（入场请出示验证码）</span>
                </div>
                <div class="ticket_ta_s" v-else-if="items.used_flag==1">
                    已使用
                    <span>（入场请出示验证码）</span>
                </div>
                <div class="ticket_ta_s" v-else>
                    已过期
                    <span>（入场请出示验证码）</span>
                </div>
                <div class="middle_line">
                    <div class="triangle-right"></div>
                    <div class="linboder"></div>
                    <div class="triangle-left"></div>
                </div>
                <div class="ticket_da" v-if="items.used_flag==0">
                    <qrcode :value="items.ticket_code" :options="{ size: 170 }"></qrcode>
                    {{getCodeNumber(items.ticket_code)}}
                </div>
                <div class="ticket_da_s" v-else-if="items.used_flag==1">
                    {{getCodeNumber(items.ticket_code)}}
                </div>
                <div class="ticket_da" v-else>
                    {{getCodeNumber(items.ticket_code)}}
                </div>
            </div>
        </div>
        <div id="banBottom" v-if="orderinfo.order_status=='SUCCESS'||orderinfo.order_status=='FINISH'">
            <div class="jinTds">
                <div>实际付款：<span>{{orderinfo.paid_amount}}.00 元</span></div>
                <div>订单编号：<span>{{orderinfo.order_id}}</span></div>
                <div>支付时间：<span>{{orderinfo.create_time}}</span></div>
                <div>订单金额：<span>{{orderinfo.order_amount}}.00 元</span></div>
                <div>代金券抵：<span>{{orderinfo.coupon_amount||0}}.00 元</span></div>
                <div>画贝抵扣：<span>{{orderinfo.balance_amount||0}}.00 元</span></div>
            </div>
        </div>
        <div id="banBottom" v-else-if="orderinfo.order_status=='CLOSED'">
            <div class="jinTds">
                <div>订单编号：<span>{{orderinfo.order_id}}</span></div>
                <div>订单金额：<span>￥{{orderinfo.order_amount}}</span></div>
            </div>
        </div>
        <div class="frieWork_e" v-show="orderinfo.order_status=='UNPAID'">
            <div class="co_moution_box_div">实际付款：</div>
            <div class="co_moution_box_Money fs18">￥{{orderinfo.order_amount-orderinfo.balance_amount-orderinfo.coupon_amount}}</div>
        </div>
        <div class="bt_ara_div_button" v-show="orderinfo.order_status=='UNPAID'" @click="rePaytheOrder()">继续支付</div>
        <div class="bt_ara_div_finish" v-show="orderinfo.order_status=='CLOSED'">已关闭</div>
    </div>
</template>

<script>
import VueQrcode from 'vue-qrcode'
export default {
    components: {
        qrcode: VueQrcode
    },
    data() {
        return {
            orderID: this.$route.query.orderID,
            orderinfo: {},
            qrCode: '',
            imgTitle: '',
            tiketStatue: '',
            tiketTime: '',
            rocallTime: 0,
        }
    },
    created() {
        this.getOrderInfo(this.orderID)
    },
    methods: {
        getCodeNumber(num) {
            try {
                return num.replace(/(.{4})/g, '$1\n');
            } catch (error) {
                console.log(error)
            }
        },
        getOrderInfo(orderid) {
            let o = {
                order_id: orderid,
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionPara', o)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');

            this.$http.get(this.$store.state.serverHost + '/yqhbsp/order/show' + getString).then(m => this.setOrderInfo(m.data)).catch(r => console.log(r));
        },
        getTimeSetForOL(date) {
            let C = new Date(date.replace(/\-/g, '/'))
            return C.getFullYear() + '年' + (1 + C.getMonth()) + '月' + C.getDate() + '日 ' + (C.getHours() < 10 ? '0' + C.getHours() : C.getHours()) + ':' + (C.getMinutes() < 10 ? '0' + C.getMinutes() : C.getMinutes())
        },
        rePaytheOrder() {
            let comInfo = {
                Event_id: this.orderID,
                OrderInfo: {
                    event_name: this.orderinfo.order_name,
                    event_time: this.orderinfo.event_time,
                    address: this.orderinfo.address,
                    venue_name: this.orderinfo.venue_name,
                    price: this.orderinfo.cost_price,
                    order_amount: this.orderinfo.order_amount,//以下为继续支付内容
                    balance_amount: this.orderinfo.balance_amount,
                    coupon_amount: this.orderinfo.coupon_amount,
                    img_title: this.imgTitle,
                    number: this.orderinfo.number,
                    cmTime: this.orderinfo.create_time.replace(/\-/g, '/')
                }
            }
            this.$store.commit('setCommodityInfo', comInfo)
            this.orderinfo.address ? this.$router.push({ path: '/pa' }) : this.$router.push({ path: '/pp' })
        },
        setOrderInfo(data) {
            if (data.errcode === 200) {

                this.orderinfo = data.record
                this.imgTitle = data.record.order_img.indexOf('http') < 0 ? this.$store.state.SyetemBaseConfig.image_prefix + data.record.order_img : '' + data.record.order_img.replace(/\http:/g,'')
                data.record.order_status == 'UNPAID' ? this.getTimeInfo() : ''
                this.tiketStatue = data.record.tickets[0].used_flag || ''
                this.tiketTime = data.record.tickets[0].used_time || ''
                try {
                    this.qrCode = data.record.tickets[0].ticket_code.replace(/(.{4})/g, '$1\n') || 0;
                } catch (error) {
                    console.log(error)
                }
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        getTimeInfo() {
            this.$store.commit('ConectionPara', {})
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/config/getSystem' + getString).then(m => this.ComputetTime(m.data.record)).catch(r => console.log(r));
        },
        ComputetTime(data) {
            let st = data.sys_time.replace(/\-/g, '/'), ct = this.orderinfo.create_time.replace(/\-/g, '/')
            let ts = new Date(st).getTime(), tc = new Date(ct).getTime()
            let cm = (15 * 60 * 1000) - (ts - tc)
            cm > 60000 ? this.rocallTime = (new Date(cm).getMinutes() < 10 ? '0' + new Date(cm).getMinutes() : new Date(cm).getMinutes()) + ':' + (new Date(cm).getSeconds() < 10 ? '0' + new Date(cm).getSeconds() : new Date(cm).getSeconds()) : this.rocallTime = '00:' + (new Date(cm).getSeconds() < 10 ? '0' + new Date(cm).getSeconds() : new Date(cm).getSeconds())
            this.runBack(cm)
        },
        runBack(cm) {
            if (cm > 0) {
                cm > 60000 ? this.rocallTime = (new Date(cm).getMinutes() < 10 ? '0' + new Date(cm).getMinutes() : new Date(cm).getMinutes()) + ':' + (new Date(cm).getSeconds() < 10 ? '0' + new Date(cm).getSeconds() : new Date(cm).getSeconds()) : this.rocallTime = '00:' + (new Date(cm).getSeconds() < 10 ? '0' + new Date(cm).getSeconds() : new Date(cm).getSeconds())
                let _msThis = this;
                setTimeout(function() {
                    cm -= 1000
                    _msThis.runBack(cm)
                }, 1000);
            } else {
                this.orderinfo.order_status = 'CLOSED'
            }
        },
        reunShow(st) {
            return this.tiketStatue == st ? true : false;
        },
        jumpToamap() {
            let pt = this.orderinfo.latitude + ',' + this.orderinfo.longitude;
            this.$router.push({ name: 'amapif', query: { lginfo: pt, lgname: this.orderinfo.address } })
        },
        jumpToACD(acNum) {
            this.$router.push({ name: 'acinfo', query: { actinfo: acNum } })
        },
    }
}
</script>
<style>
body {
    background-color: #f5f5f5;
}

#orderDetail {
    width: 100%;
    float: left;
}

#banTop {
    width: 100%;
    /* padding: 0 3.3%; */
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    background-color: #ffffff;
    flex-direction: column;
    -webkit-flex-direction: column;
    color: #333333;
}

.frieWork_e {
    float: left;
    width: 92.4%;
    padding: 3.8%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    color: #333333;
    font-weight: bold
}

.co_moution_box_div {
    float: left;
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

.co_moution_box_Money {
    color: #ff3f5c;
    font-size: 24px
}

#recomTime {
    width: 100%;
    text-align: center;
    background-color: #ff3f5c;
    color: #ffffff;
    line-height: 30px;
    height: 30px;
    font-size: 12px;
    float: left
}

.fs18 {
    font-size: 18px;
}

.ac_name {
    width: 93.4%;
    padding: 3.3%;
    font-weight: bold;
    font-size: 16px;
}

.ac_info {
    width: 93.4%;
    padding: 3.3%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    background-color: #fafafa;
}

.ac_info_img {
    width: 26.7%;
    padding-top: 26.7%;
    margin-right: 3.3%;
}

.ac_info_text {
    flex: 1;
    -webkit-flex: 1;
}

.ac_info_text div:nth-child(2) {
    font-size: 14px;
    margin: 3.3% 0;
}

.ac_ad_info {
    width: 93.4%;
    padding: 3.3%;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    font-size: 16px;
}

.ac_ad_info div {
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    justify-content: space-between;
    -webkit-justify-content: space-between;
}

.ac_ad_info img {
    height: 16px;
    margin-top: 3px;
}


.tips_img_onImg {
    align-self: flex-start;
    height: 24px;
    width: 56px;
    margin-right: -56px;
    line-height: 24px;
    text-align: center;
    background-color: #ff7869;
    color: #ffffff;
    font-size: 12px;
    position: relative;
    z-index: 2;
}























/*中间层*/

#banMiddle {
    width: 93.4%;
    padding: 3.3%;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    background-color: #f5f5f5;
}

.bt_text {
    width: 100%;
    flex: 1;
    -webkit-flex: 1;
    margin-bottom: 3.3%;
    font-size: 16px;
    color: #999999;
}

.ticket_box {
    flex: 1;
    -webkit-flex: 1;
    margin-bottom: 3.3%;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    background-color: #ffffff;
}

.ticket_ta {
    width: 93.4%;
    padding: 3.3% 3.3% 1.8% 3.3%;
    color: #333333;
    font-size: 16px;
    line-height: 24px;
}

.ticket_ta span {
    color: #999999;
}

.ticket_ta_s {
    width: 93.4%;
    padding: 3.3% 3.3% 1.8% 3.3%;
    color: #333333;
    font-size: 16px;
    line-height: 24px;
}

.ticket_ta_s span {
    font-size: 12px;
    color: #999999;
}

.middle_line {
    display: flex;
    display: -webkit-flex;
}

.triangle-right {
    width: 0;
    height: 0;
    border-top: 8px solid transparent;
    border-left: 12px solid #f5f5f5;
    border-bottom: 8px solid transparent;
}

.linboder {
    flex: 1;
    -webkit-flex: 1;
    border-bottom: 2px dashed #f5f5f5;
    margin: -5px 0 7px 0;
}

.triangle-left {
    width: 0;
    height: 0;
    border-top: 8px solid transparent;
    border-right: 12px solid #f5f5f5;
    border-bottom: 8px solid transparent;
    margin-right: -1px;
}

.ticket_da {
    width: 93.4%;
    padding: 1.8% 3.3% 3.3% 3.3%;
    color: #333333;
    font-size: 24px;
    font-weight: bold;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.ticket_da canvas {
    margin-bottom: 3.3%;
    width: 170px;
    height: 170px;
}

.ticket_da_s {
    width: 93.4%;
    padding: 1.8% 3.3% 3.3% 3.3%;
    color: #999999;
    text-decoration: line-through;
    font-size: 24px;
    font-weight: bold;
}



















/*底部框*/

#banBottom {
    margin-top: 1%;
    width: 93.4%;
    padding: 0 3.3%;
    display: flex;
    display: -webkit-flex;
    background-color: #ffffff;
    flex-direction: column;
    -webkit-flex-direction: column;
    color: #333333;
}

.jinTds {
    width: 100%;
    padding: 3.3% 0;
    line-height: 30px;
    color: #666666;
}

.jinTds div {
    display: flex;
    display: -webkit-flex;
    justify-content: space-between;
    -webkit-justify-content: space-between;
    font-size: 14px;
}


















/*.triangle-bottom {
    width: 0;
    height: 0;
    border-left: 8px solid transparent;
    border-top: 12px solid #ffffff;
    border-right: 8px solid transparent;
    margin: 3% 0 -6.1% 6.3%;
}
.triangle-bottom_bot {
    width: 0;
    height: 0;
    border-left: 8px solid transparent;
    border-top: 12px solid #dddddd;
    border-right: 8px solid transparent;
    margin: 3.3% 0 -6.3% 6.3%;
}*/

.jinDds {
    width: 93.4%;
    padding: 3.3%;
    color: #333333;
    background-color: #ffffff;
    margin-bottom: 1%;
    float: left;
}

.jinDds div {
    display: flex;
    display: -webkit-flex;
    font-size: 16px;
    margin-top: 2%;
}






/* .jinDds div:first-child{
    color: #ff3f5c;
} */

.bt_ara {
    width: 100%;
    display: flex;
    display: -webkit-flex;
    justify-content: flex-end;
    -webkit-justify-content: flex-end;
}

.bt_ara_div {
    padding: 5px 10px;
    line-height: 16px;
    font-size: 12px;
    color: #ff3f5c;
}

.bt_ara_div_finish {
    position: absolute;
    left: 0;
    bottom: 0;
    color: #666666;
    width: 100%;
    height: 40px;
    line-height: 40px;
    text-align: center;
}

.bt_ara_div_button {
    position: absolute;
    left: 0;
    bottom: 0;
    color: #ffffff;
    background-color: #ff3f5c;
    width: 100%;
    height: 46px;
    line-height: 46px;
    text-align: center;
}
</style>
