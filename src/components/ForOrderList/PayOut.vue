<template>
    <div id="mainAllOrderList" v-if="this.messageAer=='COMMON'">
        <s1t :key="'po_'+index" v-for="(item,index) in ListInfo" :eachinfo="ListInfo[index]" :type='this.messageAer'></s1t>
        <div v-show="ListInfo.length==0" style="width:100%;line-height:50px;text-align:center;color:#666666">· 你还没有退款订单哦 ·</div>
    </div>
    <div id="mainAllOrderList" v-else>
        <s2t :key="'all_'+index" v-for="(item,index) in ListInfo" :eachinfo="ListInfo[index]"></s2t>
        <div v-show="ListInfo.length==0" style="width:100%;line-height:50px;text-align:center;color:#999999">· 你还没有订单哦 ·</div>
    </div>
</template>

<script>
import s1t from './OrderBarStyle/OrderBarStyle_1.vue'
import s2t from './OrderBarStyle/OrderBarStyle_2.vue'
export default {
    data() {
        return {
            ListInfo: [],
            page_no: 1,
            page_has: 0,
            pageRoll: true
        }
    },
    created() {
        this.readList()
    },
    props: ['messageAer'],
    methods: {
        readList() {
            let r = {
                order_status: 'REFUND',
                start_time: '',
                end_time: '',
                page_no: 1,
                order_type: this.messageAer,
                page_size: 10,
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionPara', r)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/order/index' + getString).then(m => this.putInfotoList(m.data)).catch(r => console.log(r));
        },
        putInfotoList(data) {
            if (data.errcode === 200) {
                this.pageRoll = true;
                this.page_has = data.records.length;
                this.ListInfo = this.ListInfo.concat(data.records)
            } else {
                this.$toast.center(data.errmsg)
            }

        }
    },
    components: { s1t,s2t },
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
#mainAllOrderList {
    width: 100%;
    float: left;
}
</style>
