<template>
    <div id="all_in_add">
        <div id="main_info_add">
            <div class="each_lin">
                姓名
                <input v-model="nam" maxlength="50">
            </div>
            <div class="each_lin">
                联系电话
                <input v-model="tel" maxlength="11" type="tel">
            </div>
            <div class="each_lin">
                电子邮箱
                <input v-model="eml">
            </div>
            <div class="each_lin2">
                设为默认
                <div :class="[eosButton_ck]" @click="eosButton">
                    <div :class="[eosButton_div]"></div>
                </div>
            </div>
        </div>
        <div class="add_newinfo" @click="addUserContact">
            <div>确认添加</div>
        </div>
    </div>
</template>

<script>

export default {
    data() {
        return {
            eosButton_ck: 'eosButton',
            eosButton_div: 'eosButton_div',
            eosNumber: 0,
            idx: '',
            nam: '',
            tel: '',
            eml: '',
        }
    },
    created() {
        let me = JSON.parse(this.$route.query.coInfo)
        this.eosNumber = me.default_flag % 2
        this.idx = me.id
        this.nam = me.name
        this.tel = me.phone
        this.eml = me.email
    },
    methods: {
        eosButton: function () {
            this.eosButton_ck = this.eosNumber % 2 === 0 ? 'eosButton_on' : 'eosButton_do';
            this.eosButton_div = this.eosNumber % 2 === 0 ? 'eosButton_div eosButton_div_runR' : 'eosButton_div eosButton_div_runL';
            this.eosNumber++
        },
        addUserContact() {
            if (this.nam == '') {
                this.$toast.center('请输入联系人')
                return false
            }
            if (this.tel.length < 11) {
                this.$toast.center('联系方式有误')
                return false
            }
            let a = {
                id: this.idx,
                name: this.nam,
                phone: this.tel,
                email: this.eml,
                default_flag: this.eosNumber % 2,
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionParaWithOutPhone', a)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.post(this.$store.state.serverHost + '/yqhbsp/userinfo/updateContact', postString).then(m => this.addUserContactCallBack(m.data)).catch(r => this.$toast.bottom('系统错误'));
        },
        addUserContactCallBack(data) {
            if (data.errcode === 200) {
                if (this.eosNumber % 2 == 1) {
                    let m = {
                        name: this.nam,
                        phone: this.tel,
                        email: this.email
                    }
                    this.$store.commit('saveConnectionObj', m)
                }
                this.$toast.center('编辑成功')
                this.$router.go(-1)
            } else {
                this.$toast.center(data.errmsg)
            }
        }
    },
    watch: {

    },
    computed: {

    },
    mounted() {
        if (typeof (this.$route.query.idx) === 'number') {

        }
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

#main_info_add,
#all_in_add {
    float: left;
    width: 100%;
}

#all_in_add {
    min-height: 100vh;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
}

#main_info_add {
    flex: 1;
    -webkit-flex: 1;
}

.each_lin {
    float: left;
    width: 93.4%;
    padding: 2.7% 3.3%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    background-color: #ffffff;
    justify-content: space-between;
    -webkit-justify-content: space-between;
    color: #333333;
    margin-bottom: 1px;
}

.each_lin input {
    float: right;
    width: 71%;
    line-height: 32px;
    color: #333333;
    height: 32px;
    border: 0;
    padding: 0 2%;
    font-size: 16px;
    padding: 0;
    margin: 0;
    outline: none
}

.each_lin2 {
    float: left;
    width: 93.4%;
    padding: 2.7% 3.3%;
    margin-top: 1%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    background-color: #ffffff;
    justify-content: space-between;
    -webkit-justify-content: space-between;
    color: #333333;
    margin-bottom: 1px;
}



.eosButton {
    width: 56px;
    background-color: #ededed;
    height: 30px;
    -webkit-border-radius: 30px;
    border-radius: 30px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

.eosButton_on {
    width: 56px;
    background-color: #4BD48D;
    height: 30px;
    -webkit-border-radius: 30px;
    border-radius: 30px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    -webkit-animation: selectOnpay 0.5s;
}

@-webkit-keyframes selectOnpay {
    from {
        background-color: #ededed
    }
    to {
        background-color: #4BD48D
    }
}

.eosButton_do {
    width: 56px;
    background-color: #ededed;
    height: 30px;
    -webkit-border-radius: 30px;
    border-radius: 30px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    -webkit-animation: selectDopay 0.5s;
}

@-webkit-keyframes selectDopay {
    from {
        background-color: #4BD48D
    }
    to {
        background-color: #ededed
    }
}

.eosButton_div {
    width: 26px;
    margin: 2px;
    background-color: #ffffff;
    height: 26px;
    -webkit-border-radius: 30px;
    border-radius: 30px;
    box-shadow: 0 1px 2px 1px #aaaaaa
}

.eosButton_div_runR {
    margin-left: 28px;
    box-shadow: 0 1px 1px 1px #5ae57e;
    -webkit-animation: RunOnpay 0.3s;
}

@-webkit-keyframes RunOnpay {
    from {
        margin-left: 0
    }
    to {
        margin-left: 26px
    }
}

.eosButton_div_runL {
    box-shadow: 0 1px 1px 1px #aaaaaa;
    -webkit-animation: RunDopay 0.3s;
}

@-webkit-keyframes RunDopay {
    from {
        margin-left: 26px
    }
    to {
        margin-left: 0
    }
}

.add_newinfo {
    float: left;
    width: 100%;
    background-color: #ff3f5c;
    color: #ffffff;
    display: flex;
    display: -webkit-flex;
    padding: 3.3% 0;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
    font-size: 16px;
    font-weight: bold;
}
</style>
