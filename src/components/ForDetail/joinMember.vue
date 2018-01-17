<template>
    <div class="mainLips">
        <div class="tt_top">以下小伙伴已经参加</div>
        <div class="manti">
            <div class="eachMenber" v-bind:key="item" v-for="(item, index) in connect">
                <img :src="item.avatar.replace(/\http:/g,'')">
                <div>{{item.nickname}}</div>
            </div>
        </div>
        <div class="bb_aon" @click="PayOrder">
            我要报名
        </div>
    </div>
</template>

<script>
export default {
    created(){
        this.getJoinMember()
    },
    data() {
        return {
            canuse: function (statue) {
                return statue ? 'useNum_y' : 'useNum_n'
            },
            connect:[],
        }
    },
    methods: {
        PayOrder: function () {
            this.$router.go(-1)
        },
        getJoinMember() {
            let J = {
                id: this.$route.query.actinfo
            }
            this.$store.commit('ConectionPara', J)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/event/showPaidUser' + getString).then(m => this.setJoinMenber(m.data)).catch(r => console.log(r));
        },
        setJoinMenber(data) {
            if (data.errcode === 200) {
                this.connect = data.records
            } else {
                this.$toast.center(data.errmsg)
            }
        },
    }
}
</script>

<style>
body {
    background-color: #f5f5f5
}

.mainLips {
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    width: 100%;
    min-height: 100vh;
}

.tt_top {
    padding: 3.3%;
    background-color: #ffffff;
}

.manti {
    flex: 1;
    -webkit-flex: 1;
}

.eachMenber {
    background-color: #ffffff;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    padding: 3.3%;
    margin-top: 1px;
}

.eachMenber img {
    width: 15%;
    -webkit-border-radius: 100px;
    border-radius: 100px;
}

.eachMenber div {
    font-size: 16px;
    margin-left: 3.6%;
}

.bb_aon {
    float: left;
    width: 100%;
    height: 48px;
    line-height: 48px;
    background-color: #ff3f5c;
    color: #ffffff;
    text-align: center
}
</style>
