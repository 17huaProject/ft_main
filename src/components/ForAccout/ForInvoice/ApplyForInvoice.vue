<template>
    <div id="ApplyForInvoiceArea">
        <div id="ApplyForInvoiceTitle">
            <img src="../../../../static/obj_img/accout_img/invoice_invoice_icon.png">
            <div>
                <div>发票金额：
                    <span>￥{{ttMoney}}</span>
                </div>
                <div>发票类型：普通增值税专用发票</div>
            </div>
        </div>
        <div id="ApplyForInvoiceContent">
            <div id="AFIC_tapChange">
                <div :class="Barchange('com')" @click="statueOnBar='com'">公司</div>
                <div :class="Barchange('pvt')" @click="statueOnBar='pvt'">个人</div>
            </div>
            <div class="AFIC_incon_line">抬头：
                <input placeholder="请输入发票抬头" v-model="invoiceArryInfo.title">
            </div>
            <div class="AFIC_incon_line" v-show="this.statueOnBar=='com'">税号：
                <input placeholder="请输入税号" v-model="invoiceArryInfo.company_code">
            </div>
            <div class="AFIC_incon_line" v-show="this.statueOnBar=='com'">注册地址：
                <input placeholder="请输入注册地址" v-model="invoiceArryInfo.company_info">
            </div>
            <div class="AFIC_incon_line" v-show="this.statueOnBar=='com'">开户银行名称：
                <input placeholder="请输入开户银行名称" v-model="invoiceArryInfo.bank_name">
            </div>
            <div class="AFIC_incon_line" v-show="this.statueOnBar=='com'">开户银行账号：
                <input placeholder="请输入开户银行账号" v-model="invoiceArryInfo.bank_card">
            </div>
        </div>
        <div id="ApplyForInvoiceGetWay" v-if="false">
            <div class="AFIGW_T_O">现场领取</div>
            <div class="AFIGW_T_S">快递邮寄</div>
        </div>
        <div class="ApplyForInvoiceGetWay_local">发票将由工作人员在活动当天带往现场，请自行索取。</div>
        <div class="ApplyForInvoiceGetWay_mails" v-if="false">
            <div id="AFIGW_Title">
                收件信息
            </div>
            <div class="AFIC_incon_line">收件人：
                <input placeholder="填写收件人">
            </div>
            <div class="AFIC_incon_line">联系电话：
                <input placeholder="填写联系电话">
            </div>
            <div class="AFIC_incon_line">收件地址：
                <input placeholder="请输入详细收件地址">
            </div>
            <div class="ApplyForInvoiceGetWay_mails_text">快递邮寄：货到付款，
                <span>你需要支付本次快递费用</span>。一起画不承担所有发票快递费用。</div>
        </div>
        <div id="UpdataApplyForInvoice" @click="ApplyTheInvoice">提交申请</div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            statueOnBar: 'com',
            invoiceArryInfo: {
                title:'',
                company_code:'',
                company_info:'',
                bank_name:'',
                bank_card:'',
            },
            chargeArr: this.$store.state.invoicelistInfo.chargeArr || [],
            chusOrder: this.$store.state.invoicelistInfo.chusOrder || [],
            ttMoney: 0
        }
    },
    created() {
        this.getInvoiceUsrInfo()
    },
    methods: {
        Barchange(m) {
            return this.statueOnBar == m ? 'AFIC_T_S' : 'AFIC_T_O'
        },
        BarExpress(m) {
            return this.statueOnBar == m ? 'AFIGW_T_O' : 'AFIGW_T_S'
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
        getInvoiceUsrInfo() {
            let u = {
                access_token: this.$store.state.usrInfomation.access_token || ''
            }
            this.$store.commit('ConectionPara', u)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/invoice/showUserData' + getString).then(m => this.setList(m.data)).catch(r => console.log(r));
            let nun = 0;
            for (let i = 0; i < this.chargeArr.length; i++) {
                nun += this.chargeArr[i]
            }
            this.ttMoney = nun;
        },
        setList(data) {
            if (data.errcode == 200) {
                data.record!=null?this.invoiceArryInfo = data.record:''
            } else {
                this.$toast.bottom(data.errmsg)
            }
        },
        ApplyTheInvoice() {
            if (this.statueOnBar == 'com') {
                if (this.invoiceArryInfo.title == '' || this.invoiceArryInfo.company_code == '' || this.invoiceArryInfo.company_info == '' || this.invoiceArryInfo.bank_name == '' || this.invoiceArryInfo.bank_card == '') {
                    this.$toast.center('发票信息不完整')
                    return false
                } else if (this.ttMoney == 0 || this.chusOrder == []) {
                    this.$toast.center('订单选择有误')
                    this.$router.go(-1)
                    return false
                }
            } else {
                if (this.invoiceArryInfo.title == '') {
                    this.$toast.center('请填写抬头')
                    return false
                }
            }
            let r = {
                order_type: 'COMMON',
                invoice_type: this.statueOnBar == 'com' ? 'TAXSPE' : 'PERSON',
                pattern: 1,
                title: this.invoiceArryInfo.title,
                company_code: this.invoiceArryInfo.company_code,
                company_info: this.invoiceArryInfo.company_info,
                bank_name: this.invoiceArryInfo.bank_name,
                bank_card: this.invoiceArryInfo.bank_card,
                shipping_mode: 'ONSITE',
                amount: this.ttMoney,
                order_ids: this.chusOrder.length > 1 ? this.chusOrder.join(',') : this.chusOrder[0],
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionPara', r)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/invoice/add' + getString).then(m => this.putInfotoList(m.data)).catch(r => console.log(r));
        },
        putInfotoList(data) {
            if (data.errcode === 200) {
                let c = {
                    chargeArr: [],
                    chusOrder: []
                }
                this.$store.commit('saveInvoiceListInfo', c)
                this.$toast.center('开票成功')
                this.$router.go(-1)
            } else {
                this.$toast.center(data.errmsg)
            }

        },
        routerGo: function(paths) {
            
        },
    },
    watch: {
        chargeArr() {

        }
    },
    mounted() {
        let _this = this;
        // 设置一个开关来避免重负请求数据  
        window.addEventListener('scroll', function() {
            if (document.body.scrollTop + window.innerHeight >= document.body.offsetHeight) {
                if (_this.pageRoll && _this.page_has == 10) {
                    _this.pageRoll = false
                    _this.page_no++
                    _this.readList()
                }
            }
        });
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

#ApplyForInvoiceArea {
    width: 100%;
    float: left;
    color: #666666;
    position: relative;
    padding-bottom: 66px;
    font-size: 14px;
}

#ApplyForInvoiceTitle {
    padding: 2% 3.3%;
    width: 93.4%;
    display: flex;
    align-items: center;
    background-color: #ffffff;
    margin-bottom: 1.8%;
}

#ApplyForInvoiceTitle img {
    width: 30px;
    height: 30px;
    margin-right: 3.3%;
}

#ApplyForInvoiceTitle div {
    flex: 1;
}

#ApplyForInvoiceTitle span {
    color: #ff3f5c;
}

#ApplyForInvoiceTitle div div {
    line-height: 30px;
    height: 30px;
}

#ApplyForInvoiceContent {
    width: 100%;
    float: left;
    margin-bottom: 1.8%;
}

#AFIC_tapChange {
    width: 100%;
    display: flex;
}

#ApplyForInvoiceGetWay {
    width: 100%;
    float: left;
    display: flex;
}

.AFIC_T_S {
    flex: 1;
    text-align: center;
    padding: 10px 0;
    line-height: 20px;
    height: 20px;
    border-bottom: 1px solid #eeeeee;
    background-color: #ffffff;
    color: #ff3f5c;
}

.AFIC_T_O {
    flex: 1;
    text-align: center;
    padding: 10px 0;
    line-height: 20px;
    height: 20px;
    border-bottom: 1px solid #eeeeee;
    background-color: #eeeeee;
}

.AFIGW_T_S {
    flex: 1;
    text-align: center;
    padding: 10px 0;
    line-height: 20px;
    height: 20px;
    border-bottom: 1px solid #ff3f5c;
    background-color: #ffffff;
    color: #ff3f5c;
}

.AFIGW_T_O {
    text-align: center;
    padding: 10px 20px;
    line-height: 20px;
    height: 20px;
    border-bottom: 1px solid #ffffff;
    background-color: #ffffff;
}

.AFIC_incon_line {
    padding: 3.3%;
    width: 93.4%;
    display: flex;
    align-items: center;
    line-height: 20px;
    height: 20px;
    background-color: #ffffff;
    margin-bottom: 1px;
}

.AFIC_incon_line input {
    flex: 1;
    border: 0;
    outline: none;
    padding: 0 0 0 3%;
    color: #666666;
    font-size: 14px;
    line-height: 20px;
    height: 20px;
}

::-webkit-input-placeholder {
    color: #cccccc;
}

.ApplyForInvoiceGetWay_local {
    padding: 3.3%;
    width: 93.4%;
    float: left;
    line-height: 20px;
    font-size: 14px;
}

.ApplyForInvoiceGetWay_mails {
    width: 100%;
    float: left;
}

#AFIGW_Title,
.ApplyForInvoiceGetWay_mails_text {
    padding: 3.3%;
    width: 93.4%;
    float: left;
    line-height: 20px;
    font-size: 14px;
}

.ApplyForInvoiceGetWay_mails_text span {
    color: #ff3f5c;
}

#UpdataApplyForInvoice {
    position: fixed;
    bottom: 0;
    left: 0;
    font-size: 16px;
    width: 100%;
    height: 56px;
    line-height: 56px;
    text-align: center;
    background-color: #ff3f5c;
    color: #ffffff;
    float: left;
}
</style>
