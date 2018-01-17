<template>
    <div id="OrdertoinvoicelistArea">
        <div class="LisSpas_topBot">
            <div>开票金额为实际付款金额，抵用券、礼品卡余额暂不支持开票</div>
            <div @click="routerGo('/invLst')">开票历史</div>
        </div>
        <div class="LisSpas_otoi">
            <div style="text-align:center;margin:3.3% 0;" v-if="ListInfo.length==0">暂无订单可以开具发票</div>
            <!-- <div class="Lis_month_area">&nbsp;</div> -->
            <div class="Lis_ThisMonthOrder" :key="'vol'+index" v-for="(item,index) in ListInfo" @click="arrayInItem(item.order_id,item.paid_amount)">
                <!-- :key="'l_tmo'+index" v-for="(item,index) in ListInfo" -->
                <img class="order_select_invoice" :src="selectOrderInlist(item.order_id)">
                <div class="order_invoice_img" :style="{background: 'url(\''+ getImgAddr(item.order_img)+'\')'+ 'center center no-repeat',backgroundSize:'auto 100%'}"></div>
                <div class="order_invoice_context">
                    <div class="oic_ac_name">{{item.order_name}}</div>
                    <div class="oic_ac_creatTime">创建时间：{{item.order_name}}</div>
                    <div class="oic_spc_line"></div>
                    <div class="oic_number">发票金额：
                        <span>￥{{item.paid_amount}}</span>
                    </div>
                </div>
            </div>
        </div>
        <div id="OrderTOInvoiceBottom">
            <div id="otib_content" @click="getAllin()">
                <div><img class="order_select_invoice" :src="ListInfo.length!=0&&ListInfo.length==chusOrder.length?'../../../../static/obj_img/copy_img/buy_contacts_icon_1.png':'../../../../static/obj_img/copy_img/buy_contacts_icon_2.png'">全选</div>
                <div>
                    <span>{{chusOrder.length}} </span>个订单 共
                    <span>{{ttMoney}}</span> 元</div>
            </div>
            <div id="otiv_button" @click="routerGo('/invApy')">下一步</div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            ListInfo: [],
            chargeArr: this.$store.state.invoicelistInfo.chargeArr || [],
            page_no: 1,
            page_size: 20,
            pageRoll: true,
            chusOrder: this.$store.state.invoicelistInfo.chusOrder || [],
            ttMoney: 0
        }
    },
    created() {
        this.getInvoiceList()
    },
    methods: {
        arrayInItem(item, num) {
            let indexID = this.chusOrder.indexOf(item);
            if (indexID < 0) {
                this.chusOrder.push(item)
                this.chargeArr.push(num)
            } else {
                this.chusOrder.splice(indexID, 1)
                this.chargeArr.splice(indexID, 1)
            }
            let nun = 0;
            for (let i = 0; i < this.chargeArr.length; i++) {
                nun += this.chargeArr[i]
            }
            this.ttMoney = nun;
        },
        getAllin() {
            this.chargeArr = [];
            this.chusOrder = [];
            for (let i = 0; i < this.ListInfo.length; i++) {
                this.chusOrder.push(this.ListInfo[i].order_id)
                this.chargeArr.push(this.ListInfo[i].paid_amount)
            }
            let nun = 0;
            for (let i = 0; i < this.chargeArr.length; i++) {
                nun += this.chargeArr[i]
            }
            this.ttMoney = nun;
        },
        selectOrderInlist(idn) {
            return this.chusOrder.indexOf(idn) >= 0 ? '../../../../static/obj_img/copy_img/buy_contacts_icon_1.png' : '../../../../static/obj_img/copy_img/buy_contacts_icon_2.png'
        },
        getImgAddr(add) {
            return add.indexOf('http') < 0 ? this.$store.state.SyetemBaseConfig.image_prefix + add : '' + add.replace(/\http:/g,'')
        },
        getInvoiceList() {
            let InvoListInfo = {
                order_type:'COMM',
                page_no:1,
                page_size:20,
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionPara', InvoListInfo)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/invoice/indexOpenOrder' + getString).then(m => this.setList(m.data)).catch(r => console.log(r));
        },
        setList(data) {
            if (data.errcode == 200) {
                this.ListInfo = data.records
            } else {
                this.$toast.bottom(data.errmsg)
            }

        },
        putInfotoList(data) {
            if (data.errcode === 200) {
                this.pageRoll = true;
                this.page_has = data.records.length;
                this.ListInfo = this.ListInfo.concat(data.records)
            } else {
                this.$toast.center(data.errmsg)
            }

        },
        routerGo: function(paths) {
            let c = {
                chargeArr: this.chargeArr,
                chusOrder: this.chusOrder
            }
            this.$store.commit('saveInvoiceListInfo', c)
            if (paths == '/invApy') {
                this.chargeArr.length == 0 ? this.$toast.center('你还没有选择订单') : this.$router.push({ path: paths })
            } else {
                this.$router.push({ path: paths })
            }
        },
    },
    watch: {
        chargeArr() {

        }
    },
    mounted() {

    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

#OrdertoinvoicelistArea,
.LisSpas_otoi {
    width: 100%;
    float: left;
}

.Lis_month_area {
    width: 93.4%;
    padding: 3.3%
}

.Lis_ThisMonthOrder {
    display: flex;
    align-items: center;
    background-color: #ffffff;
    padding: 5% 0;
    width: 100%;
    margin-bottom: 1.8%;
}

.order_select_invoice {
    width: 30px;
    height: 30px;
    padding: 5% 2%;
}

.order_invoice_statue {
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

.order_invoice_img {
    width: 24.7%;
    padding-top: 24.7%;
    margin-right: 3.3%;
}

.order_invoice_context {
    display: flex;
    flex-direction: column;
    flex: 1;
    margin-right: 3.3%;
}

.oic_ac_name,
.oic_ac_creatTime {
    font-size: 12px;
    margin-bottom: 3%;
    color: #666666;
}

.oic_spc_line {
    width: 100%;
    height: 1px;
    background-color: #eeeeee;
    margin-top: 3%;
    margin-bottom: 6%;
}

.oic_number {
    margin-bottom: 3%;
    color: #333333;
    text-align: right;
}

.oic_number span {
    color: #ff3f5c
}

#OrderTOInvoiceBottom {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 80px;
    background-color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

#otiv_button {
    width: 120px;
    height: 50px;
    line-height: 50px;
    background-color: #ff3f5c;
    color: #ffffff;
    text-align: center;
    margin-right: 10px;
    border-radius: 3px;
}

#otib_content {
    flex: 1;
    height: 60px;
    margin-left: 10px;
    color: #666666;
}

#otib_content div:first-child {
    display: flex;
    align-items: center;
    font-size: 16px;
    height: 38px;
}

#otib_content div:first-child img {
    width: 30px;
    height: 30px;
}

#otib_content div:last-child {
    font-size: 12px;
    height: 22px;
    line-height: 22px;
}

#otib_content div:last-child span {
    color: #ff3f5c;
}

.LisSpas_topBot {
    width: 100%;
    display: flex;
    font-size: 14px;
    align-items: center;
}

.LisSpas_topBot div:first-child {
    border-right: 1px solid #eeeeee;
    font-size: 12px;
}

.LisSpas_topBot div:last-child {
    width: 80px;
    text-align: center
}

.LisSpas_topBot div {
    margin: 2.3% 0;
    padding: 1% 3.3%;
}
</style>
