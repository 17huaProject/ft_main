<template>
    <div id="ReceiveCardArea">
        <swiper :options="swiperOption">
            <!-- <div class="NextPage_info" slot="button-next">关于一起画</div> -->
            <swiper-slide>
                <div id="ReceiveCard_R_Title">关于一起画</div>
                <img id="ReceiveCard_L_conner" src="//image.17hua.me/upload/image/201708/1504144210419112.png">
                <img id="ReceiveCard_R_Img" src="//image.17hua.me/upload/image/201708/1504144210604367.png">
                <div id="ReceiveCard_R_Context">
                    hi~
                    <div>你的朋友送了你一张“一起画”专属Gift卡，快点去接收参加绘画派对。</div>
                    <!-- <ul>
                                    <li>一起画，一个
                                        <span>创意绘画平台</span>，这是创意油画家的派对。零基础下有着无限的创意，
                                        <span>边吃、边喝、边画</span>。感受真挚的快乐，画出心中的色彩。</li>
                                    <li>Gift卡，可在一起画平台支付任意商品活动，领取后，可在
                                        <span>“我得账户”-“我得钱包”</span>进行查询并使用。</li>
                                </ul> -->
                    <span>￥{{CardInfo.price}}</span>
                </div>
                <div id="ReceiveCard_CardInfo_2">
                    <span>一起画Gift卡</span>
                </div>
            </swiper-slide>
            <swiper-slide>
                <img id="ReceiveCard_MainImg" src="../../../static/obj_img/giftcard_img/card_pc.png">
                <div id="ReceiveCard_MainText">
                    <!-- <div>To:000</div>
                                <div>context info a brown fox jump over the lazy dog</div>
                                <div>By:111</div> -->
                    无论你是否接触过油画，在这里你都将创作出
                    <span>独一无二</span>的作品，参加这场属于绘画的派对。零基础，低难度，边吃、边玩、边画。<br> 领取Gift卡后请
                    <span>长按二维码关注一起画</span>，进入“我的账户”-“我的钱包”查看明细。
                </div>
                <div id="ReceiveCard_CardInfo">
                    <img class="ReceiveCard_CardInfo_Pict" src="../../../static/obj_img/pays_img/qrCode_in_paySuccess.png">
                    <div class="ReceiveCard_CardInfo_Mass">
                        <img src="../../../static/obj_img/giftcard_img/logo.png">
                        <div>人人都是艺术家</div>
                        <div>长按二维码关注一起画</div>
                    </div>
                </div>
            </swiper-slide>
        </swiper>
        <div id="ReceiveCard_ComfireGet_o" v-show="!onlead" >正在领取 <i class="el-icon-loading"></i></div>
        <div id="ReceiveCard_ComfireGet" v-if="CardInfo.gift_status=='PAID'" v-show="onlead" @click="resCard()">确认领取</div>
        <div id="ReceiveCard_Later" v-else>已过期...</div>
        <div id="ReceiveCard_QRandMessage" v-show="fiblur">
            <!-- @click="SetbackGround(false)" -->
            <div id="ReceiveCard_QRandMessageBox" class="animated fadeInUp">
                <img id="ReceiveCard_QRandMessageBox_img" src="../../../static/obj_img/pays_img/qrCode_in_paySuccess.png">
                <div id="ReceiveCard_QRandMessageBox_txt">
                    <img src="../../../static/obj_img/pays_img/paysuc_icon.png">领取成功
                </div>
                <div id="ReceiveCard_QRandMessageBox_context">
                    恭喜你，
                    <span>请长按二维码进入一起画</span>，在
                    <span>“我的账户”-“我的钱包”</span>查看明细。
                </div>
            </div>
        </div>
        <div id="ReceiveCard_LaterMessage" v-show="later">
            <div id="ReceiveCard_LaterMessageBox">
                <img id="ReceiveCard_LaterMessageBox_img" src="//image.17hua.me/upload/image/201708/1504144210420441.png">
                <div id="ReceiveCard_LaterMessageBox_context">
                    hi~<br> 你的朋友送了你一张“一起画”专属Gift卡，可惜你来迟了。
                </div>
                <div id="ReceiveCard_Iknow" @click="later=false">我知道了</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            swiperOption: {
                effect: 'flip',
                grabCursor: true,
                nextButton: '.swiper-wrapper',
                loop: true
            },
            fiblur: false,
            later: false,
            CardInfo: '',
            onlead:true
        }
    },
    created() {
        
        let cr = {
            sender_id: this.$route.query.u_id,
            gift_id: this.$route.query.g_id
        }
        this.$store.commit('ConectionPara', cr)
        let getString = this.$store.state.getParamString;
        let postString = getString.replace('?', '');
        this.$http.get(this.$store.state.serverHost + '/yqhbsp/gift/show' + getString).then(m => this.getInfoGiftCard(m.data)).catch(r => this.$toast.bottom(r));
    },
    methods: {
        getInfoGiftCard(data) {
            this.later = data.record.gift_status != 'PAID' ? true : false;
            this.CardInfo = data.record
        },
        resCard() {
            this.onlead = false;
            let dr = {
                sender_id: this.$route.query.u_id,
                gift_id: this.$route.query.g_id,
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionPara', dr)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/gift/receiveGift' + getString).then(m => this.getMess(m.data)).catch(r => this.$toast.bottom(r));
        },
        getMess(data){
            if(data.errcode==200){
                document.getElementById('ReceiveCard_ComfireGet_o').innerHTML = '领取成功'
                this.fiblur = true;
            }else{
                document.getElementById('ReceiveCard_ComfireGet_o').innerHTML = '抱歉:'+errmsg
                this.$toast.bottom(errmsg)
            }
        },
        mounted() {
            this.$store.state.usrInfomation.access_token == ""? this.$router.repalce({ path: "/bdv" }): "";
        }
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

#ReceiveCardArea {
    float: left;
    width: 100%;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.swiper-container {
    float: left;
    width: 90%;
    margin: 5% 5% 20% 5%;
}

.bosr {
    -webkit-filter: blur(10px);
    filter: blur(10px);
}

.NextPage_info {
    position: absolute;
    right: 0;
    top: -10%;
    width: 90%;
    margin: 5%;
    text-align: right;
    color: #ff604f;
    text-decoration: underline;
}

.swiper-slide {
    width: 100%;
    float: left;
    background-color: #ffffff;
    box-shadow: 0 1px 2px 1px #999;
    border-radius: 3px;
}

#ReceiveCard_MainImg {
    width: 86%;
    margin: 7%;
}

#ReceiveCard_MainText {
    width: 86%;
    margin: 0 7%;
    color: #666666;
    text-align: justify;
    text-justify: distribute-all-lines;
    line-height: 20px;
    font-size: 14px;
}

#ReceiveCard_MainText span {
    color: #ff604f;
}



#ReceiveCard_CardInfo {
    width: 86%;
    font-size: 12px;
    color: #666666;
    padding: 7%;
    display: flex;
    justify-content: space-between;
}

#ReceiveCard_CardInfo_2 {
    width: 90%;
    margin-top: 5%;
    font-size: 12px;
    color: #666666;
    padding: 2% 5%;
    border-top: 1px solid #eeeeee;
    display: flex;
    justify-content: flex-end;
}

#ReceiveCard_R_Title {
    width: 92%;
    position: absolute;
    right: 0;
    top: 0;
    font-size: 14px;
    width: 100%;
    text-align: right;
    text-decoration: underline;
    padding: 3%;
    color: #ff604f;
}

.ReceiveCard_CardInfo_Pict {
    width: 80px;
    height: 80px;
    margin-right: 5%;
}

.ReceiveCard_CardInfo_Mass {
    flex: 1;
}

.ReceiveCard_CardInfo_Mass img {
    height: 40px;
}

.ReceiveCard_CardInfo_Mass div {
    height: 20px;
}

#ReceiveCard_L_conner {
    width: 100%;
}

#ReceiveCard_R_Img {
    width: 80%;
    margin: 0 10% 5% 10%;
}

#ReceiveCard_R_Context {
    width: 90%;
    margin: 5% 5% 7% 5%;
}

#ReceiveCard_R_Context div {
    font-size: 14px;
    line-height: 20px;
    margin: 2% 0;
}

#ReceiveCard_R_Context ul {
    list-style-type: disc;
    -webkit-margin-before: 0;
    -webkit-margin-after: 0;
    -webkit-margin-start: 0px;
    -webkit-margin-end: 0px;
    -webkit-padding-start: 5%;
}

#ReceiveCard_R_Context li {
    font-size: 14px;
    color: #666666;
    margin-bottom: 5%;
    text-align: justify;
    text-justify: distribute-all-lines;
}

#ReceiveCard_R_Context span {
    font-size: 24px;
    font-weight: bold;
    float: right;
    color: #ff604f;
    margin-top: 10%;
}

#ReceiveCard_ComfireGet {
    position: fixed;
    left: 0;
    bottom: 0;
    color: #ffffff;
    background-color: #ff604f;
    width: 100%;
    height: 46px;
    line-height: 46px;
    text-align: center;
    z-index: 10;
}
#ReceiveCard_ComfireGet_o{
    position: fixed;
    left: 0;
    bottom: 0;
    color: #666666;
    background-color: #999999;
    width: 100%;
    height: 46px;
    line-height: 46px;
    text-align: center;
    z-index: 10;
}
#ReceiveCard_Later {
    position: fixed;
    left: 0;
    bottom: 0;
    color: #666666;
    width: 100%;
    height: 46px;
    line-height: 46px;
    text-align: center;
    z-index: 10;
}

#ReceiveCard_QRandMessage {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.8);
    z-index: 100;
    display: flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
}

#ReceiveCard_QRandMessageBox {
    width: 90%;
    background-color: #fff;
    border-radius: 5px;
}

#ReceiveCard_QRandMessageBox_img {
    width: 40%;
    margin: 10% 30% 0 30%;
}

#ReceiveCard_QRandMessageBox_txt {
    width: 100%;
    padding: 2% 0;
    display: flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
    color: #666666;
}

#ReceiveCard_QRandMessageBox_txt img {
    height: 20px;
    margin-right: 2%;
}

#ReceiveCard_QRandMessageBox_context {
    width: 80%;
    padding: 3% 10% 10% 10%;
    text-align: justify;
    text-justify: distribute-all-lines;
    color: #666666;
    line-height: 30px;
}

#ReceiveCard_QRandMessageBox_context span {
    color: #ff604f;
}

#ReceiveCard_LaterMessage {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.8);
    z-index: 100;
    display: flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
}

#ReceiveCard_LaterMessageBox {
    width: 90%;
    background-color: #fff;
    border-radius: 5px;
}

#ReceiveCard_LaterMessageBox_img {
    width: 40%;
    margin: 10% 30% 0 30%;
}

#ReceiveCard_LaterMessageBox_txt {
    width: 100%;
    padding: 2% 0;
    display: flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
    color: #666666;
}

#ReceiveCard_LaterMessageBox_txt img {
    height: 20px;
    margin-right: 2%;
}

#ReceiveCard_LaterMessageBox_context {
    width: 80%;
    padding: 3% 10% 10% 10%;
    text-align: justify;
    text-justify: distribute-all-lines;
    color: #666666;
    line-height: 30px;
}

#ReceiveCard_LaterMessageBox_context span {
    color: #ff604f;
}

#ReceiveCard_Iknow {
    width: 40%;
    margin: 0 30% 8% 30%;
    text-align: center;
    background-color: #ff604f;
    color: #ffffff;
    line-height: 40px;
    height: 40px;
    border-radius: 3px;
}
</style>
