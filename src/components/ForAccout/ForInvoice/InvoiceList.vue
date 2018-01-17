<template>
    <div id="InvoiceListArea">
        <div class="InvoiceAreaBox" @click="goInvDetail(item.id)" :key="'ivolc'+index" v-for="(item,index) in infoListReceive">
            <div class="InvoiceTimeStatue">
                <div>{{item.create_time}}</div>
                <div v-if="item.invoice_status=='FINISH'">已签收</div>
                <div v-else-if="item.invoice_status=='HANDLED'">开票成功</div>
                <div v-else-if="item.invoice_status=='EXPRESS'">正在寄送</div>
                <div else>处理中</div>
            </div>
            <div class="InvoiceContent">
                <div>发票类型：{{item.invoice_type=='TAXSPE'?'增值税专用发票':'个人'}}</div>
                <div>抬头：{{item.title}}</div>
                <!-- <div>发票号：{{item.id}}</div> -->
                <div>价税合计：{{item.amount}}元</div>
            </div>
            <div class="InvoiceButton">
                <div>发票详情</div>
            </div>
        </div>
        <!-- <div class="InvoiceAreaBox">
            <div class="InvoiceTimeStatue">
                <div>2017-06-15</div>
                <div>申请成功</div>
            </div>
            <div class="InvoiceContent">
                <div>发票类型：增值税专用发票</div>
                <div>抬头：一袋信息科技有限公司</div>
                <div>发票号：2948334233X</div>
                <div>价税合计：89元</div>
            </div>
            <div class="InvoiceButton">
                <div>发票详情</div>
            </div>
        </div> -->
    </div>
</template>

<script>
export default {
    data() {
        return {
            InvoListInfo: {
                page_no: 1,
                page_size: 20,
                access_token: this.$store.state.usrInfomation.access_token || ''
            },
            infoListReceive:{}
        }
    },
    created(){
        this.getInvoiceList()
    },
    methods: {
        getInvoiceList() {
            this.$store.commit('ConectionPara', this.InvoListInfo)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/invoice/index' + getString).then(m => this.setList(m.data)).catch(r => console.log(r));
        },
        setList(data){
            if(data.errcode==200){
                this.infoListReceive = data.records
            }else{
                this.$toast.bottom(data.errmsg)
            }
        },
        goInvDetail: function(ivid) {
            this.$router.push({ name: 'invd', query: { iv_id: ivid } })
        },
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

#InvoiceListArea {
    width: 100%;
    float: left;
    font-size: 14px;
}

.InvoiceAreaBox {
    width: 100%;
    float: left;
    color: #666666;
    margin-bottom: 1.8%;
}

.InvoiceTimeStatue {
    float: left;
    width: 93.4%;
    padding: 3.3%;
    background-color: #ffffff;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1px;
}

.InvoiceTimeStatue div:last-child {
    color: #ff3f5c;
}

.InvoiceContent {
    float: left;
    width: 93.4%;
    padding: 3.3%;
    background-color: #ffffff;
    margin-bottom: 1px;
}

.InvoiceContent div {
    line-height: 30px;
    height: 30px;
    width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
}

.InvoiceButton {
    float: left;
    width: 100%;
    background-color: #ffffff;
}

.InvoiceButton div {
    border: 1px solid #eeeeee;
    padding: 3px 5px;
    margin: 3.3%;
    float: right;
    border-radius: 2px;
    font-size: 14px;
}
</style>
