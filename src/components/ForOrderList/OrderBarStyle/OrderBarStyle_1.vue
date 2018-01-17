<template>
    <div class="orderpills">
        <div class="orderTitle">{{eachinfo.order_name}}</div>
        <div class="mainOrderinfo" @click="routertoDetail()">
            <div class="leftImg" :style="{background: 'url(\''+ this.getImgAddr(eachinfo.order_img)+'\')'+ 'center center no-repeat',backgroundSize:'auto 100%'}">
                <!--eachinfo.order_img.indexOf('http')<0?this.$store.state.SyetemBaseConfig.image_prefix+eachinfo.order_img:''+eachinfo.order_img-->
                <div v-if="eachinfo.order_status=='SUCCESS'" style="background-color:#ff3f5c">可使用</div>
                <div v-else-if="eachinfo.order_status=='UNPAID'" style="background-color:#FDA649">未支付</div>
                <div v-else-if="eachinfo.order_status=='FINISH'" style="background-color:#20B14B">已使用</div>
                <div v-else-if="eachinfo.order_status=='REFUND'" style="background-color:#C29B70">已退款</div>
                <div v-else-if="eachinfo.order_status=='CLOSED'" style="background-color:#C29B70">已关闭</div>
            </div>
            <div class="rightInfo">
                <div>时间：{{getTimeSetForOL(eachinfo.event_time)}}</div>
                <div style="font-size:12px">地址：{{eachinfo.address}}</div>
                <div>{{eachinfo.number}} 张票
                    <span style="color:#999999;margin:0 16px;">|</span>总价：￥{{eachinfo.order_amount}}</div>
            </div>
        </div>
        <div class="endButton">
            <div v-if="eachinfo.order_status=='SUCCESS'" @click="routertoDetail()">立即使用</div>
            <div v-else-if="eachinfo.order_status=='UNPAID'" @click="routertoDetail()">去支付</div>
            <div v-else-if="eachinfo.order_status=='PENDING'" @click="routertoDetail()">已完成</div>
        </div>
    </div>
</template>
<script>

export default {
    methods: {
        getImgAddr(add) {
            return add.indexOf('http') < 0 ? this.$store.state.SyetemBaseConfig.image_prefix + add : '' + add.replace(/\http:/g,'')
        },
        routertoDetail() {
            this.$router.push({ name: 'ordta', query: { orderID: this.eachinfo.order_id } })
        },
        getTimeSetForOL(date) {
            try {
                let C = new Date(date.replace(/\-/g, '/'))
                return C.getFullYear() + '年' + (1 + C.getMonth()) + '月' + C.getDate() + '日 ' + (C.getHours() < 10 ? '0' + C.getHours() : C.getHours()) + ':' + (C.getMinutes() < 10 ? '0' + C.getMinutes() : C.getMinutes())
            } catch (error) {
                console.log(error)
            }

        }
    },
    props: ['eachinfo']
}
</script>

<style>
.orderpills {
    width: 100%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    flex-direction: column;
    -webkit-flex-direction: column;
    background-color: #ffffff;
    margin-top: 1%;
}

.orderTitle {
    width: 93.4%;
    padding: 3.3%;
    flex: 1;
    -webkit-flex: 1;
}

.mainOrderinfo {
    width: 93.4%;
    padding: 3.3%;
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    background-color: #fafafa;
}

.leftImg {
    width: 28.3%;
    padding-bottom: 28.3%;
    margin-right: 3.3%;
    background-size: contain
}

.leftImg div {
    height: 24px;
    width: 56px;
    line-height: 24px;
    margin-bottom: -24px;
    text-align: center;
    background-color: #ff7869;
    color: #ffffff;
    font-size: 12px;
}

.rightInfo {
    flex: 1;
    -webkit-flex: 1;
    height: 100%;
    color: #666666;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    justify-content: space-between;
    -webkit-justify-content: space-between;
}

.rightInfo div {
    flex: 1;
    -webkit-flex: 1;
    padding: 3.3% 0;
}

.endButton {
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    width: 93.4%;
    padding: 3.3%;
    justify-content: flex-end;
    -webkit-justify-content: flex-end;
    min-height: 16px;
}

.endButton div {
    font-size: 16px;
    width: 70px;
    text-align: center;
    padding: 3px 5px;
    color: #ff604f;
    border: 1px solid #ff604f;
    -webkit-border-radius: 3px;
    border-radius: 3px;
}
</style>