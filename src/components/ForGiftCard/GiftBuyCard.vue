<template>
    <div id="GiftBuyCardArea">
        <div id="GiftCardBuyT">
            <img src="//image.17hua.me/upload/image/201708/1504144210604367.png">
        </div>
        <div id="GiftCardBuyChargeArea">
            <div id="GiftCardBuyChargeTitle">选择金额</div>
            <div id="GiftCardBuyChargeInfo">
                <div :key="'ma'+index" :class="OnselectThis(index)" v-for="index in chargeArray" @click="SelectThisBarC(index)">￥{{index}}</div>
                <input placeholder="自定义金额" v-model="OnselectThisBar" maxlength="10" type="tel">
            </div>
        </div>
        <div id="GiftCardBuyChargeRulesMss">我已阅读
            <span>礼品卡购买规则</span>
            <img src="../../../static/obj_img/pop_right_icon.png">
        </div>
        <div id="nextStep_GiftCardBuy_c" class="nextStep_GiftCardBuy_can" v-show="OnselectThisBar!=0&&payStatue" @click="ConfirmToPay">确认支付</div>
        <div id="nextStep_GiftCardBuy_n" class="nextStep_GiftCardBuy_cant" v-show="OnselectThisBar==0&&payStatue">确认支付</div>
        <div id="nextStep_GiftCardBuy_o" class="nextStep_GiftCardBuy_cant" v-show="OnselectThisBar!=0&&!payStatue">正在支付
            <i class="el-icon-loading"></i>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            chargeArray: [99, 199, 299, 399, 499, 599],
            OnselectThisBar: this.$store.state.GiftCardCustInfo.num || 0,
            payStatue: true
        }
    },
    created() {
        if (typeof WeixinJSBridge == "undefined") {
            if (document.addEventListener) {
                document.addEventListener('WeixinJSBridgeReady', this.onBridgeReady, false);
            } else if (document.attachEvent) {
                document.attachEvent('WeixinJSBridgeReady', this.onBridgeReady);
                document.attachEvent('onWeixinJSBridgeReady', this.onBridgeReady);
            }
        } else {
            this.onBridgeReady();
        }
    },
    methods: {
        OnselectThis(num) {
            return this.OnselectThisBar === num ? 'GiftCardChargeSelectOn' : 'GiftCardChargeSelectNormal'
        },
        SelectThisBarC(index) {
            this.OnselectThisBar = index;
        },
        ConfirmToPay() {
            // if (confirm('确认支付' + this.OnselectThisBar + '元么？')) {
            //     let mesSate = {};
            //     mesSate.page = this.$store.state.GiftCardCustInfo.page;
            //     mesSate.to = this.$store.state.GiftCardCustInfo.to;
            //     mesSate.context = this.$store.state.GiftCardCustInfo.context;
            //     mesSate.by = this.$store.state.GiftCardCustInfo.by;
            //     mesSate.num = this.OnselectThisBar;
            //     this.$store.commit('saveGiftCardStyleinfo', mesSate)
            //     this.$router.push({ path: '/gcsd' })
            // } else {
            //     this.$toast.bottom('取消支付')
            // }
            this.payStatue = false;
            let cr = {
                sender: 'To用户',
                content: '边吃边喝边画画',
                receiver: '一起画',
                photo_id: '',
                pay_channel: 'WXPAY',
                price: this.OnselectThisBar,
                number: 1,
                order_amount: this.OnselectThisBar,
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionPara', cr)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/gift/add' + getString).then(m => this.onBridgeReady(m.data)).catch(r => this.$toast.bottom(r));
        },
        onBridgeReady(arr) {
            var _this = this;
            if (arr.errcode == 400) {
                this.$toast.center(arr.errmsg)
                this.payStatue = true
                return false
            }
            let Pi = {
                order_id: arr.record.gift_id,
                paid_amount: arr.record.paid_amount,
                pay_time: arr.record.pay_time
            }
            this.$store.commit('saveGiftCardOrderInfo', Pi)
            if (arr.errcode == 200) {
                WeixinJSBridge.invoke(
                    'getBrandWCPayRequest', arr.record.pay_info,
                    function(res) {
                        if (res.err_msg == "get_brand_wcpay_request:ok") {// 使用以上方式判断前端返回,微信团队郑重提示：res.err_msg将在用户支付成功后返回    ok，但并不保证它绝对可靠。 
                            _this.$router.replace({ name: 'gcsdp', query: { g_id: arr.record.gift_id,u_id:_this.$store.state.usrInfomation.user_id } })
                        } else {
                             this.payStatue = true
                        }
                    }
                );
            } else {
                this.payStatue = true
                //_this.$router.replace({ name: 'payCop', query: { s: 'f' } })
                this.$toast.center(arr.errmsg)
            }
        },
    }
}
</script>

<style>
#GiftBuyCardArea {
    width: 100%;
    float: left;
}

#GiftCardBuyT {
    width: 70%;
    padding: 10px 15%;
    float: left;
    background-color: #ffffff;
}

#GiftCardBuyT img {
    width: 100%;
    float: left;
}

#GiftCardBuyChargeArea {
    width: 90%;
    padding: 0 5% 10px 5%;
    float: left;
    background-color: #ffffff;
}

#GiftCardBuyChargeTitle {
    width: 100%;
    float: left;
    padding-bottom: 10px;
    background-color: #ffffff;
}

#GiftCardBuyChargeInfo {
    width: 90%;
    padding: 10px 5% 0 5%;
    float: left;
    /* display: flex;
    display: -webkit-flex; */
    /* flex-flow: row wrap;
    -webkit-flex-flow: row wrap;
    align-content: flex-start;
    -webkit-align-content: flex-start; */
}

#GiftCardBuyChargeInfo div {
    float: left;
    width: 21%;
    margin: 0 2% 5% 2%;
    line-height: 16px;
    height: 16px;
    text-align: center;
    padding: 2% 0;
    border-radius: 5px;
}

.GiftCardChargeSelectOn {
    box-shadow: 0 0 0 1px #ff3f5c;
    color: #ff3f5c;
}

.GiftCardChargeSelectNormal {
    box-shadow: 0 0 0 1px #cccccc;
    color: #666666;
}

#GiftCardBuyChargeInfo input {
    float: left;
    width: 40%;
    padding: 2%;
    line-height: 16px;
    height: 16px;
    margin: 0 2% 5% 2%;
    -webkit-appearance: none;
    border: 1px solid #cccccc;
    outline: none;
    border-radius: 5px;
    color: #666666;
    font-size: 16px;
}

#nextStep_GiftCardBuy_c {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 46px;
    line-height: 46px;
    text-align: center;
}

#nextStep_GiftCardBuy_o {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 46px;
    line-height: 46px;
    text-align: center;
}

#nextStep_GiftCardBuy_n {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 46px;
    line-height: 46px;
    text-align: center;
}

.nextStep_GiftCardBuy_can {
    color: #ffffff;
    background-color: #ff3f5c;
}

.nextStep_GiftCardBuy_cant {
    color: #666666;
    background-color: #eeeeee;
}

#GiftCardBuyChargeRulesMss {
    float: left;
    width: 90%;
    padding: 10px 5% 0 5%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

#GiftCardBuyChargeRulesMss span {
    color: #ff3f5c;
}

#GiftCardBuyChargeRulesMss img {
    height: 16px;
    margin-left: 5px;
}
</style>
