<template>
    <div id="manObar">
        <img id="usrImgGet" src="//image.17hua.me/upload/image/201711/jcw427jwmbewgwfzasoz9z6assnvpbt9.png">
        <div id="usrText">手机验证码登陆<br>与我们一起画</div>
        <div id="phoneLineSet">
            <input v-model="phoneSet" type="tel" placeholder="请输入手机号" maxlength="11">
            <img v-show="phoneSet.length===11" src="../../../static/obj_img/pop_right_icon.png">
        </div>
        <div v-show="showtIPS[0]" class="errMsgShow" style="margin-top:38%;">
            <img src="../../../static/obj_img/pop__wrong_icon.png">{{MsG_1}}</div>
        <div id="smsCodelineSet">
            <div id="smsArea">
                <input v-model="smscodeSet" type="tel" placeholder="请输入验证码" maxlength="6">
                <img v-show="smscodeSet.length===6" src="../../../static/obj_img/pop_right_icon.png">
            </div>
            <div id="smsButton" @click="smsSend">{{sendButton}}</div>
        </div>
        <div v-show="showtIPS[1]" class="errMsgShow" style="margin-top:63%;">
            <img src="../../../static/obj_img/pop__wrong_icon.png">{{MsG_2}}</div>
        <div id="sendSMS" @click="upLoad">开启一起画</div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            MsG_1: '手机号错误',
            MsG_2: '验证码错误',
            showtIPS: [false, false, false, false],
            phoneSet: '',
            smscodeSet: '',
            sendButton: '获取验证码',
            sentTime: 60,
        }
    },
    created() {

    },
    methods: {
        smsSend() {
            if (this.phoneSet.length < 11) {
                this.showtIPS = [true, false]
            } else {
                this.showtIPS[0] = false;
                if (this.sentTime == 60) {
                    this.runTime();
                }
            }
        },
        runTime() {
            if (this.sentTime == 60) {
                this.sentTime--;
                this.sendButton = this.sentTime + '秒后重发'
                setTimeout(this.runTime, 1000);
                this.sendSMScode();
            } else if (this.sentTime == 0) {
                this.sentTime = 60
                this.sendButton = '重新发送'
            } else {
                this.sentTime--;
                this.sendButton = this.sentTime + '秒后重发'
                setTimeout(this.runTime, 1000)
            }
        },
        upLoad() {
            if (this.phoneSet.length < 11 || this.smscodeSet.length < 6) {
                this.showtIPS = [this.phoneSet.length < 11, this.smscodeSet.length < 6]
            } else {
                var WXHS = {
                    phone: this.phoneSet,
                    login_type: 'WX',
                    open_id: this.$store.state.openID,
                    sms_code: this.smscodeSet,
                    referer:this.$store.state.invitedUserId
                }
                this.$store.commit('ConectionParaWithOutPhone', WXHS)
                let getString = this.$store.state.getParamString;
                let postString = getString.replace('?', '');
                this.$http.get(this.$store.state.serverHost + '/yqhbsp/user/registerAndBind' + getString).then(m => this.runGoback(m.data)).catch(r => console.log(r));
            }
        },
        sendSMScode() {
            let setSMS = {
                phone: this.phoneSet
            }
            this.$store.commit('ConectionParaWithOutPhone', setSMS)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/sms/sendRegister' + getString).then(m => this.$toast.bottom('验证码已发送')).catch(r => console.log(r));
        },
        runGoback(data) {
            if (data.errcode == 200) {
                this.$toast.bottom('欢迎来到一起画')
                this.$store.commit('getBaseUsrInfo', data.record.login_user)
                this.$router.go(-1)
            } else {
                this.$toast.center(data.errmsg)
            }
        }
    },
    watch: {

    },
    computed: {

    }
}
</script>

<style>
#manObar {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    background: url('//image.17hua.me/upload/image/201711/eb0vbzqlj5moccpt1lht5abz2q5fa60k.jpg') no-repeat;
    background-size: cover;
}

#usrImgGet {
    position: absolute;
    left: 50%;
    top: 0;
    width: 140px;
    margin: 15% -70px;
}

#usrText {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    margin: 28% 0;
    text-align: center;
    color: #ffffff;
}

#phoneLineSet {
    position: absolute;
    left: 0;
    top: 80px;
    width: 86%;
    padding: 0 2%;
    margin: 25% 5%;
    border-bottom: 1px solid #ffffff;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

#phoneLineSet input {
    border: 0;
    padding: 11px 0;
    flex: 1;
    -webkit-flex: 1;
    outline: none;
    font-size: 16px;
    background: rgba(0, 0, 0, 0);
    color: #ffffff;
}

::-webkit-input-placeholder {
    color: #cfcfcf;
}

#phoneLineSet img {
    height: 20px;
    padding: 10px 0;
}

#smsCodelineSet {
    position: absolute;
    left: 0;
    top: 80px;
    width: 90%;
    padding: 0;
    margin: 50% 5%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: space-between;
    -webkit-justify-content: space-between;
}

#smsArea {
    padding: 0 2%;
    width: 180px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    border-bottom: 1px solid #ffffff;
}

#smsArea input {
    border: 0;
    padding: 11px 0;
    flex: 1;
    -webkit-flex: 1;
    outline: none;
    font-size: 16px;
    background: rgba(0, 0, 0, 0);
    color: #ffffff;
}

#smsArea img {
    height: 20px;
    padding: 10px 0;
}

#smsButton {
    padding: 0 0 0 5%;
    color: #ffffff;
    text-align: center;
}

#sendSMS {
    position: absolute;
    left: 0;
    top: 80px;
    width: 80%;
    padding: 2% 0;
    margin: 80% 10% 0 10%;
    text-align: center;
    background-color: #ff3f5c;
    color: #ffffff;
    border-radius: 30px;
}

.errMsgShow {
    position: absolute;
    left: 0;
    top: 80px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    width: 86%;
    padding: 0;
    margin: 0 7%;
    color: #ff3f5c;
    font-size: 14px;
}

.errMsgShow img {
    height: 14px;
    margin-right: 2.4%;
}
</style>
