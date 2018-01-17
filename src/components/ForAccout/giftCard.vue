<template>
    <div class="giftcard_area">
        <div id="giftARA">
            <img src="../../../static/obj_img/giftcard_img/gift_photo.png">
            <div>电子Gift卡</div>
            <ul>
                <li>自定义礼品卡金额</li>
                <li>DIY心意贺卡</li>
                <li>在线将最棒的礼物和心意送给好友</li>
            </ul>
            <button @click="ToSetGiftCard">去送卡</button>
        </div>
        <div id="giftOrderLine" v-show="false" @click="ToSetGiftList">
            Gift卡订单
            <img src="../../../static/obj_img/list_icon_arrRight.png">
        </div>
    </div>
</template>

<script>
export default {
    methods:{
        ToSetGiftCard(){
            this.$router.push({path:'/gcst'})
        },
        ToSetGiftList(){
            this.$router.push({path:'/gol'})
        }
    }
}
</script>

<style>
.giftcard_area,
#giftARA {
    width: 100%;
    float: left;
}

#giftARA {
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    flex-direction: column;
    -webkit-flex-direction: column;
    background-color: #ffffff;
}

#giftARA img {
    width: 75%;
    padding: 10% 12.5%;
}

#giftARA div {
    width: 100%;
    text-align: center;
    color: #666666;
    font-weight: bold;
}

#giftARA ul {
    width: 80%;
    padding: 0 10%;
    color: #666666;
}

#giftARA ul li {
    line-height: 29px;
    font-size: 16px;
}

#giftARA button {
    width: 60%;
    line-height: 40px;
    height: 40px;
    margin: 6% 20% 14% 20%;
    font-size: 16px;
    background-color: #ff3f5c;
    color: #ffffff;
    border: 0;
    outline: none;
    -webkit-border-radius: 4px;
    border-radius: 4px;
}

#giftOrderLine {
    float: left;
    width: 93.4%;
    padding: 3.3%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    background-color: #ffffff;
    margin-top: 1.6%;
    justify-content: space-between;
    -webkit-justify-content: space-between;
}

#giftOrderLine img {
    height: 14px;
}
</style>
