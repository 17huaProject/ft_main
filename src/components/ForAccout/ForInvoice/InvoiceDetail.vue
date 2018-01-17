<template>
    <div id="InvoiceDetailArea">
        <div id="InvoiceDetailTopArea">
            <div v-if="infoDetailReceive.invoice_status=='FINISH'">开票状态：已签收</div>
            <div v-else-if="infoDetailReceive.invoice_status=='HANDLED'">开票状态：开票成功</div>
            <div v-else-if="infoDetailReceive.invoice_status=='EXPRESS'">开票状态：正在寄送</div>
            <div else>开票状态：处理中</div>
            <div v-show="false">发票已经寄出，顺丰单号：294444444444444</div>
        </div>
        <div id="InvoiceDetailmaithosn">
            <div class="InvoiceDetailmaithosn_line">
                <img src="../../../../static/obj_img/accout_img/invoice_order_icon.png">
                <div class="InvoiceDetailmaithosn_DIV">
                    <div>发票金额：￥{{infoDetailReceive.amount}}</div>
                    <div>发票类型：{{infoDetailReceive.invoice_type=='TAXSPE'?'增值税专用发票':'个人'}}</div>
                </div>
            </div>
            <div class="InvoiceDetailmaithosn_line">
                <img src="../../../../static/obj_img/accout_img/invoice_time_icon.png">
                <div class="InvoiceDetailmaithosn_DIV">
                    <div>开票时间：{{infoDetailReceive.create_time}}</div>
                </div>
            </div>
            <div class="InvoiceDetailmaithosn_line">
                <img src="../../../../static/obj_img/accout_img/invoice_invoice_icon.png">
                <div class="InvoiceDetailmaithosn_DIV">
                    <div>发票抬头：{{infoDetailReceive.title}}</div>
                    <div v-show="infoDetailReceive.invoice_type=='TAXSPE'">税号：{{infoDetailReceive.company_code}}</div>
                    <div v-show="infoDetailReceive.invoice_type=='TAXSPE'">注册地址：{{infoDetailReceive.company_info}}</div>
                    <div v-show="infoDetailReceive.invoice_type=='TAXSPE'">开户银行名称：{{infoDetailReceive.bank_name}}</div>
                    <div v-show="infoDetailReceive.invoice_type=='TAXSPE'">开户银行账号：{{infoDetailReceive.bank_card}}</div>
                </div>
            </div>
        </div>
        <div id="InvoiceDetailOrderList" v-if="false">
            <div id="InvoiceDetailOrderListButton">
                相关订单<img src="../../../../static/obj_img/list_more_xiala_icon.png">
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            infoDetailReceive: {}
        }
    },
    created() {
        this.getInvoiceList()
    },
    methods: {
        getInvoiceList() {
            let InvoDetailReq = {
                invoice_id: this.$route.query.iv_id,
                access_token: this.$store.state.usrInfomation.access_token || ''
            }
            this.$store.commit('ConectionPara',InvoDetailReq)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/invoice/show' + getString).then(m => this.setDetail(m.data)).catch(r => console.log(r));
        },
        setDetail(data) {
            if (data.errcode == 200) {
                this.infoDetailReceive = data.record
            } else {
                this.$toast.bottom(data.errmsg)
            }
        }
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

#InvoiceDetailArea {
    float: left;
    width: 100%;
    color: #666666;
    font-size: 14px;
}

#InvoiceDetailTopArea {
    width: 93.4%;
    padding: 4.3% 3.3%;
    background-color: #ffffff;
    margin-bottom: 1.8%;
    float: left;
}

#InvoiceDetailmaithosn {
    float: left;
    width: 100%;
    margin-bottom: 1.8%;
}

#InvoiceDetailTopArea div:first-child {
    color: #ff3f5c;
}

#InvoiceDetailTopArea div:last-child {
    margin-top: 2%;
    font-size: 14px;
}

.InvoiceDetailmaithosn_line {
    float: left;
    width: 93.4%;
    padding: 3.3%;
    background-color: #ffffff;
    margin-bottom: 1px;
    display: flex;
    align-items: center;
}

.InvoiceDetailmaithosn_line img {
    width: 30px;
    height: 30px;
    margin-right: 3.3%
}

.InvoiceDetailmaithosn_DIV {
    flex: 1;
    flex-direction: column;
}

.InvoiceDetailmaithosn_DIV div {
    line-height: 30px;
    width: 100%;
}

#InvoiceDetailOrderList {
    float: left;
    width: 100%;
}

#InvoiceDetailOrderListButton {
    float: left;
    width: 93.4%;
    padding: 3.3%;
    background-color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

#InvoiceDetailOrderListButton img {
    height: 20px;
}
</style>
