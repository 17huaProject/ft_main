<template>
    <div class="ps_main">
        <div class="tiket_bar" v-show="!this.cheackFrom()">
            <div class="top_line"></div>
            <!-- 支付成功 关注-->
            <div class="ticket_box" v-show="this.statue==='s'">
                <div class="title_area">
                    <img src="../../static/obj_img/pays_img/pay_success_icon.png">
                    <div v-if="this.$store.state.commInfo.OrderInfo.name">购买成功</div>
                    <div v-else>出票成功</div>
                </div>
                <div class="order_info">
                    <div class="order_info_title">{{ative_name}}</div>
                    <div class="order_info_detail" v-if="total_price==0" style="color:#ff3f5c:font-size:12px;">使用余额支付</div>
                    <div class="order_info_detail" v-else>实付款：￥{{total_price}}</div>
                    <div class="order_info_detail">订单号：{{orderid}}</div>
                    <div class="order_info_detail">购买时间：{{paytime}}</div>
                </div>
            </div>
            <!-- 调用失败 关注-->
            <div class="ticket_box" v-show="this.statue==='f'">
                <div class="title_area">
                    <div>支付失败</div>
                </div>
                <div class="order_info">
                    <div class="order_info_title">{{ative_name}}</div>
                    <div class="order_info_detail">待付款：￥0</div>
                    <div class="order_info_detail">订单号：无</div>
                </div>
            </div>
        </div>
        <div class="tiket_bar" v-show="this.cheackFrom()">
            <div class="top_line"></div>
            <!-- 支付成功 未关注-->
            <div class="ticket_box" v-show="this.statue==='s'">
                <div class="title_area_o">
                    <img class="title_area_o_img" src="../../static/obj_img/pays_img/qrCode_in_paySuccess.png">
                    <div class="title_area_o_div" v-if="this.$store.state.commInfo.OrderInfo.name">
                        <div>
                            <img src="../../static/obj_img/pays_img/paysuc_icon.png">购买成功
                        </div>
                        <div>长按二维码关注一起画微信公众号，查看商品订单</div>
                    </div>
                     <div class="title_area_o_div" v-else>
                        <div>
                            <img src="../../static/obj_img/pays_img/paysuc_icon.png">出票成功
                        </div>
                        <div>长按二维码关注一起画微信公众号，开启活动提醒</div>
                    </div>
                </div>
                <div class="order_info_o">
                    <div class="order_info_title">{{ative_name}}</div>
                    <div class="order_info_detail">实付款：￥{{total_price}}</div>
                    <div class="order_info_detail">订单号：{{orderid}}</div>
                    <div class="order_info_detail">购买时间：{{paytime}}</div>
                </div>
            </div>
            <!-- 失败 未关注-->
            <div class="ticket_box" v-show="this.statue==='f'">
                <div class="title_area_o">
                    <img class="title_area_o_img" src="../../static/obj_img/pays_img/qrCode_in_paySuccess.png">
                    <div class="title_area_o_div" >
                        <div>
                            支付失败
                        </div>
                        <div>长按二维码关注一起画微信公众号，查看详细信息</div>
                    </div>
                </div>
                <div class="order_info_o">
                    <div class="order_info_title">{{ative_name}}</div>
                    <div class="order_info_detail">待付款：￥0</div>
                    <div class="order_info_detail">订单号：无</div>
                </div>
            </div>
        </div>
        <div class="button_area"  v-show="this.statue==='s'">
            <!-- <div class="Cheack_order" @click="routertoDetail">查看订单</div> -->
            <div class="Cheack_lottery" @click="routertoDetail">查看订单</div>
            <!-- <div class="Cheack_lottery" @click="GoDetail">再来一单</div> -->
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            statue: this.$route.query.s,
            ative_name: this.$store.state.commInfo.OrderInfo.event_name||this.$store.state.commInfo.OrderInfo.name,
            total_price: this.$store.state.payOrderInfo.paid_amount,
            orderid: this.$store.state.payOrderInfo.order_id,
            paytime: this.$store.state.payOrderInfo.pay_time
        }
    },
    methods: {
        routertoDetail() {
            this.$router.push({ name: 'ordtp', query: { orderID: this.$store.state.payOrderInfo.order_id } })
        },
        GoDetail() {
            this.$router.go(-1)
        },
        cheackFrom() {
            return sessionStorage.getItem('inWechat') || false
        }
    }
}
</script>

<style>
body {
    background-color: #f5f5f5
}

.ps_main {
    float: left;
    width: 100%;
}

.tiket_bar {
    float: left;
    width: 92.8%;
    margin: 9.6% 3.8%;
}

.top_line {
    float: left;
    width: 100%;
    padding-bottom: 4%;
    border-radius: 20px;
    -webkit-border-radius: 20px;
    background-color: #000000;
}

.ticket_box {
    float: left;
    background-color: #ffffff;
    width: 94%;
    margin: -1.8% 3%;
    box-shadow: 0 2px 3px 1px #ededed
    /*background-image:-webkit-gradient(radial,10% 10,5,10% 5 ,17,from(#ff33dd),color-stop(10%, #d86707),color-stop(10%, #FFF),to(#FFF));*/
}

.title_area,
.order_info,
.button_area {
    float: left;
    width: 100%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
    border-bottom: 1px solid #f2f2f2;
}

.title_area {
    padding: 15% 0;
    background: -webkit-linear-gradient(top, rgba(222, 222, 222, 0.5) 5%, rgba(255, 255, 255, 0.8) 20%, rgba(255, 255, 255, 1) 90%);
}

.order_info {
    flex-direction: column;
    -webkit-flex-direction: column;
    margin-top: 12%;
    padding-bottom: 12%;
}

.title_area img {
    width: 16%;
    margin-right: 3%
}

.title_area div {
    font-size: 24px;
    font-weight: bold;
    color: #999999
}


.title_area_o {
    float: left;
    width: 90%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
    border-bottom: 1px solid #f2f2f2;
    padding: 10% 5% 5% 5%;
    background: -webkit-linear-gradient(top, rgba(222, 222, 222, 0.5) 5%, rgba(255, 255, 255, 0.8) 20%, rgba(255, 255, 255, 1) 90%);
}

.title_area_o_img {
    float: left;
    width: 30%;
    margin-right: 5%
}

.title_area_o_div {
    float: left;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    justify-content: space-between;
    -webkit-justify-content: space-between;
    flex: 1;
    -webkit-flex: 1;
    font-size: 16px;
    color: #333333
}

.title_area_o_div div {
    float: left;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    flex: 1;
    -webkit-flex: 1;
    -webkit-align-items: center;
}

.title_area_o_div div:first-child {
    margin-bottom: 5%;
    font-weight: bold;
    color: #666666
}

.title_area_o_div div img {
    float: left;
    margin-right: 5%;
    height: 20px;
}

.order_info_o {
    float: left;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    width: 100%;
    flex: 1;
    -webkit-flex: 1;
    margin-top: 12%;
    padding-bottom: 12%;
}



.order_info_title {
    width: 86%;
    margin: 0 7% 3% 7%;
    font-size: 18px;
}

.order_info_detail {
    width: 86%;
    margin: 0 7% 3% 7%;
    font-size: 16px;
}

.Cheack_order {
    width: 33.8%;
    text-align: center;
    border: 1px solid #ff3f5c;
    color: #ff3f5c;
    background-color: #ffffff;
    padding: 5px 0;
    -webkit-border-radius: 3px;
    border-radius: 3px;
    font-size: 16px;
}

.Cheack_lottery {
    width: 80%;
    text-align: center;
    border: 1px solid #ff3f5c;
    color: #ffffff;
    background-color: #ff3f5c;
    padding: 5px 0;
    -webkit-border-radius: 3px;
    border-radius: 3px;
    font-size: 16px;
}
</style>
