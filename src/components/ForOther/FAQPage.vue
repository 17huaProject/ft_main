<template>
    <div id="FQAPageArea">
        <div class="FQALine tt_co_s">参加活动</div>
        <div class="FQALine ct_co" @click="lin1=!lin1">Q：如何报名/支付？<img :src="imgBack(lin1)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin1">A：进入活动列表页，点击任意活动→进入活动详情页→点击“买单”按钮进行报名/支付。</div>

        <div class="FQALine ct_co" @click="lin2=!lin2">Q：一起画支持哪些支付？<img :src="imgBack(lin2)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin2">A：一起画目前仅支持微信支付、后续会开发其他支付方式，敬请期待。</div>

        <div class="FQALine ct_co" @click="lin3=!lin3">Q：如何退款？<img :src="imgBack(lin3)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin3">A： 参与者如涉及退款业务时，请拨打客服电话021-67991260，并提供参与的活动单号、报名时填写的手机号及姓名。工作人员将在核实订单之后，操作退款，退款金额会在1-5个工作日按原渠道退还至参与者支付的账户中。温馨提示：活动开始前48小时不支持退款，已使用的优惠劵不予退还，感谢谅解。</div>

        <div class="FQALine ct_co" @click="lin4=!lin4">Q：如何确认已经购票成功？<img :src="imgBack(lin4)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin4">A：“进入一起画”→“我的账户”→“我的订单”→点击任意订单查看订单详情→查看订单状态显示“可使用”即为购票成功。</div>

        <div class="FQALine ct_co" @click="lin5=!lin5">Q：参与者到现场如何使用电子票进场？<img :src="imgBack(lin5)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin5">A：“我的账户”→“我的订单”→点击任意订单查看订单详情→查看二维码→活动现场工作人员扫码验证信息，即可使用电子票进场。</div>

        <div class="FQALine ct_co" @click="lin6=!lin6">Q：参与者到现场如何使用电子票进场？<img :src="imgBack(lin6)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin6">A：“我的账户”→“我的订单”→点击任意订单查看订单详情→查看二维码→活动现场工作人员扫码验证信息，即可使用电子票进场。</div>

        <div class="FQALine tt_co_s">专属活动</div>

        <div class="FQALine ct_co" @click="lin7=!lin7">Q：如何定制一个专属活动？<img :src="imgBack(lin7)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin7">A：“我的账户”→“定制活动”→点击“我要定制”填写信息表单→“提交表单”按钮发布活动，我们的工作人员会在24小时内核对信息和活动日程后联系您。</div>

        <div class="FQALine ct_co" @click="lin8=!lin8">Q：如何查看定制活动状态？<img :src="imgBack(lin8)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin8">A：“我的账户”→“我的订单”→“活动定制订单”按钮→查看已发布活动定制状态。</div>

        <div class="FQALine ct_co" @click="lin9=!lin9">Q：一起画定制活动流程是什么？<img :src="imgBack(lin9)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin9">A：参与者在线提交定制活动信息，提交后，我们的工作人员会在24小时内核对信息和活动日程并联系您，电话和您沟通活动举办的时间和场地，沟通完毕后，系统会自动在“我的订单“生成一个该活动的支付按钮，点击按钮进行活动费用支付，活动当天，我们的专业策划会准时前往现场进行布置，专业画室团队进行指导绘画直至活动结束。</div>

        <div class="FQALine tt_co_s">电子卡</div>

        <div class="FQALine ct_co" @click="lin10=!lin10">Q：电子卡是什么？<img :src="imgBack(lin10)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin10">A：电子礼品卡是一起画平台专属电子通用卡，可以送朋友，送亲人，送同事，并在线制作DIY心意贺卡，将满满的心意和创意送给TA，获得礼品卡后，领取者可以在一起画报名任意绘画活动，并通过礼品余额支付。</div>

        <div class="FQALine ct_co" @click="lin11=!lin11">Q：如何购买电子卡？<img :src="imgBack(lin11)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin11">A：“我的账户”→“定制Gift卡”→选择“礼品卡金额并支付”→“分享给好友”。</div>  
        
        <div class="FQALine ct_co" @click="lin12=!lin12">Q：如何购买并赠送电子卡？<img :src="imgBack(lin12)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin12">A：“我的账户”→“定制Gift卡”→选择“礼品卡金额并支付”→“分享给好友”。</div>  
        
        <div class="FQALine ct_co" @click="lin13=!lin13">Q：收到电子去哪儿看？<img :src="imgBack(lin13)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin13">A：好友收到电子卡后，点击领取，届时卡内金额会直接进入该好友账户余额，并在下单的时候勾选使用即可。</div>  

        <div class="FQALine tt_co_s">倾听您的声音</div>

        <div class="FQALine ct_co" @click="lin14=!lin14">Q：如何进行意见反馈？<img :src="imgBack(lin14)"></div>
        <div class="FQALine tt_co animated fadeIn" v-show="lin14">A：一起画为了给用户提供最好的服务，设置了用户反馈渠道，你可以将你的感受，体验，建议发送给我们，我们一起让一起画变得更好。移动端→“更多”→“意见反馈”，客服邮箱：service@17hua.me</div>  

    </div>
</template>

<script>
export default {
    data() {
        return {
            lin1: false,
            lin2: false,
            lin3: false,
            lin4: false,
            lin5: false,
            lin6: false,
            lin7: false,
            lin8: false,
            lin9: false,
            lin10: false,
            lin11: false,
            lin12: false,
            lin13: false,
            lin14: false,
        }
    },
    methods: {
        imgBack(s) {
            return s ? '../../../static/obj_img/paiter_shouqi_icom.png' : '../../../static/obj_img/list_more_xiala_icon.png'
        }
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

#FQAPageArea {
    width: 100%;
    float: left;
}

.FQALine {
    padding: 3.3%;
    width: 93.4%;
    float: left;
    font-size: 14px;
    display: flex;
    align-items: center;
    justify-content: space-between;

}

.FQALine img {
    height: 20px;
}

.tt_co {
    color: #666666;
    margin-bottom: 1%;
    text-align : justify 
}

.tt_co_s {
    color: #333333;
    font-size: 16px;
    font-weight: bold;
}

.ct_co {
    color: #333333;
    background-color: #fff;
    margin-bottom: 1%;
}
</style>
