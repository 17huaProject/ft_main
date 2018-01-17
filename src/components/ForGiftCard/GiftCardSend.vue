<template>
    <div id="GiftCardSendArea">
        <div id="GiftCardSendTitleInfo" :class="onBlurSit">
            <img src="../../../static/obj_img/giftcard_img/give_icon_1.png">
            <div>一起画Gift卡 ￥{{this.$store.state.giftCardOrderInfo.paid_amount}}</div>
            <!--{{carid}}-->
        </div>
        <div id="GiftCardSendMainInfo" :class="onBlurSit">
            <div id="GiftCardSendPrompting">
                <img src="../../../static/obj_img/giftcard_img/give_share_icon.png">Gift卡购买成功！分享给好友领取吧。
            </div>
            <div id="GiftCardSendHowToDoIt" @click="onShow(true)">如何赠送？</div>
            <div id="GiftCardSendRules">
                <ul>
                    <li>你可在
                        <span>“我的账户-我的Gift卡”</span>中查看赠送详情以及其他信息</li>
                    <li>礼品卡一旦售出，
                        <span>不予退换</span>，敬请谅解</li>
                    <li>如有疑问，请关注一起画微信公众号，联系客服处理</li>
                </ul>
            </div>
        </div>
        <div id="GiftCardSendShareMethod" v-show="onBlurSit=='bosr'" @click="onShow(false)">
            <img src="//image.17hua.me/upload/image/201708/1504144210930152.png">
            <div>点击右上角<br>选择发送给“朋友”或“朋友圈”</div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            onBlurSit: '',
            carid: this.$route.query.g_id
        }
    },
    // created(){
    //     let share = {
    //         sharetitle: '￥'+this.$store.state.giftCardOrderInfo.paid_amount +  ' | 一起画 Gift卡',
    //         sharelink: 'wx.17hua.me/#/rcp?g_id=' + this.$route.query.g_id+'&u_id='+this.$route.query.u_id,
    //         shareimgUrl: document.location.protocol +'//image.17hua.me/upload/image/201709/zoud6drye2zobx7jggu629a4vrn2yttw.png',
    //         shareDesc: '送礼如心，你不来一起画么？'
    //     }
    //     this.$store.commit('useShareFun', share)
    // },
    methods: {
        onShow(statue) {
            this.onBlurSit = statue ? 'bosr' : '';
        },
    },
    mounted() {
        let share = {
            sharetitle: '￥'+this.$store.state.giftCardOrderInfo.paid_amount +  ' | 一起画 Gift卡',
            sharelink: 'https://wx.17hua.me/#/rcp?g_id=' + this.$route.query.g_id+'&u_id='+this.$route.query.u_id,
            shareimgUrl: document.location.protocol +'//image.17hua.me/upload/image/201711/a7rx181p6shok1cdgeo3r74zl1of8atk.png',
            shareDesc: '送礼如心，你不来一起画么？'
        }
        this.$store.commit('useShareFun', share)
    }
}
</script>

<style>
#GiftCardSendArea {
    width: 100%;
    float: left;
}

#GiftCardSendTitleInfo {
    width: 100%;
    padding: 9.6% 0 0 0;
    margin-bottom: 9.6%;
    float: left;
    color: #666666;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-color: #ffffff;
}

#GiftCardSendTitleInfo img {
    width: 22%;
    float: left;
}

#GiftCardSendTitleInfo div {
    padding: 5.5% 0;
    font-size: 18px;
}

#GiftCardSendMainInfo {
    width: 91.6%;
    padding: 0 4.3%;
}

#GiftCardSendPrompting {
    width: 90%;
    padding: 5%;
    float: left;
    color: #666666;
    display: flex;
    align-items: center;
}

#GiftCardSendPrompting img {
    height: 30px;
    margin-right: 5%;
}

#GiftCardSendHowToDoIt {
    width: 40%;
    margin: 9.6% 30%;
    text-align: center;
    height: 36px;
    line-height: 36px;
    float: left;
    background-color: #ff3f5c;
    color: #ffffff;
    font-size: 14px;
    border-radius: 3px;
}

#GiftCardSendRules {
    border-top: 1px solid #eeeeee;
    width: 100%;
    float: left;
    padding: 9.6% 0;
    margin-top: 5%;
}

#GiftCardSendRules li {
    margin-bottom: 18px;
    color: #666666;
}

#GiftCardSendRules li span {
    color: #ff3f5c
}

.bosr {
    -webkit-filter: blur(10px);
    filter: blur(10px);
}

#GiftCardSendShareMethod {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: hsla(0, 0%, 30%, .3);
    background: rgba(0, 0, 0, 0.8);
    z-index: 5
}

#GiftCardSendShareMethod img {
    width: 64%;
    padding: 5% 16%;
}

#GiftCardSendShareMethod div {
    width: 90%;
    padding: 0 5%;
    line-height: 30px;
    font-size: 20px;
    color: #ffffff;
    text-align: center;
}
</style>
