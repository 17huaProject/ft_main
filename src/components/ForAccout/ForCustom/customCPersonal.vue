<template>
    <div id="tamMain">
        <img class="dz_down_button" @click="jumpDown" v-show="dpButtom" src="../../../../static/obj_img/dz_bottom_icon copy.png">
        <img id="titleImg" src="//image.17hua.me/upload/image/201711/1511751621778456.png">
        <div id="manTia">
            <div>私人活动定制</div>
        </div>
        <div id="manTia_Tschild">邀请20至200位好朋友参加一场值得回忆的主题派对，举起香醇的红酒，为单身、为生日、为即将诞生的宝宝而庆祝吧！</div>
        <div class="mass_titles">一起画的活动特色</div>
        <div class="mess_Content">
            <img class="mess_img" src="//image.17hua.me/upload/image/201711/1511751622017663.png">
            <div class="mess_info">
                <div>白手套服务</div>
                <div>我们拥有最完善的娱乐绘画体系，最意想不到的创意绘画场所，而你只需确定时间，拉上小伙伴，来一场创作之旅。</div>
            </div>
        </div>
        <div class="mess_Content">
            <div class="mess_info">
                <div>100%定制化</div>
                <div>您只要选择一幅画，确定时间、地点，其他的事，就交给我们吧。</div>
            </div>
            <img class="mess_img" src="//image.17hua.me/upload/image/201711/1511751630215969.png">
        </div>
        <div class="mess_Content">
            <img class="mess_img" src="//image.17hua.me/upload/image/201711/1511751622667922.png">
            <div class="mess_info">
                <div>独特并有趣</div>
                <div>品尝着红酒，画一幅独特的油画，并与您喜欢的人愉快相处。</div>
            </div>
        </div>
        <div class="mass_info" style="border-top:10px solid #f5f5f5">
            <div class="mass_title">申请信息</div>
            <div class="mass_inputLine">
                <div class="inputTitle">联系人：</div>
                <input v-model="Upinfo.custName" maxlength="50">
            </div>
            <div class="mass_inputLine">
                <div class="inputTitle">手机号：</div>
                <input v-model="Upinfo.custphone" maxlength="11">
            </div>
            <div class="mass_inputLine">
                <div class="inputTitle">参加人数：</div>
                <input type="tel" v-model="Upinfo.joinNumber" maxlength="4" placeholder="（数字）">
            </div>
            <div class="mass_inputLine" @click="selectCalendar">
                <div class="inputTitle">活动日期：</div>
                <div class="change_mass">{{selcetTime}}</div>
                <img src="../../../../static/obj_img/list_icon_arrRight.png">
            </div>
            <div class="mass_inputLine">
                <div class="inputTitle">在哪儿举办：</div>
                <input placeholder="请尽量详细描述" v-model="Upinfo.activeAddress" maxlength="200">
            </div>
            <div class="mass_inputLine">
                <div :class="sleClassCt(this.motChage)" @click="chagn(true)">私人
                    <span v-show="motChage">(独特并有趣)</span>
                </div>
                <div :class="sleClassCt(!this.motChage)" @click="chagn(false)">团队
                    <span v-show="!motChage">(彰显竞争力)</span>
                </div>
            </div>
            <div class="mass_inputLine_red" @click="getToCrv">
                <div><span style="color:#666666">我已阅读</span>活动定制规则</div>
                <img src="../../../../static/obj_img/band_img/pop_right_icon.png">
            </div>
        </div>
        <div class="mass_bottom_red" @click="upAfcdInfo">提交定制</div>
    </div>
</template>

<script>
var gettype = Object.prototype.toString
export default {
    data() {
        return {
            da: new Date(),
            motChage: true,
            selcetTime: this.$store.state.selectTimeFomt,
            Upinfo: this.$store.state.customization,
            dpButtom: window.scrollY < 700?true:false
        }
    },
    methods: {
        routerGo: function (paths) {
            this.$router.push({ path: paths })
        },
        upAfcdInfo() {
            if (this.Upinfo.custphone === '') {
                this.$toast.center('请检查输入号码')
                return false
            }
            if (this.Upinfo.custName === '') {
                this.$toast.center('请检查联系人')
                return false
            }
            if (this.Upinfo.joinNumber === '') {
                this.$toast.center('请输入人数')
                return false
            }
            if (this.Upinfo.activeAddress === '') {
                this.$toast.center('你还没告诉我在哪儿')
                return false
            }
            if (confirm(this.motChage ? '确认提交该私人定制信息？' : '确认提交该团建活动信息？')) {
                let uptSitInfo = {
                    contact: this.Upinfo.custName,
                    phone: this.Upinfo.custphone,
                    custom_type: this.motChage ? 'PRIVATE' : 'COMPANY',
                    est_date: this.selcetTime,
                    est_num: this.Upinfo.joinNumber,
                    city: this.Upinfo.activeAddress,
                    access_token: this.$store.state.usrInfomation.access_token
                };
                this.$store.commit('ConectionParaWithOutPhone', uptSitInfo)
                let getString = this.$store.state.getParamString;
                let postString = getString.replace('?', '');
                this.$http.post(this.$store.state.serverHost + '/yqhbsp/custom/add', postString).then(m => this.getCallback(m.data)).catch(r => console.log(r));
            }
        },
        chagn(data) {
            this.motChage = data
        },
        sleClassCt(data) {
            return data ? 'mass_button_select' : 'mass_button'
        },
        getCallback(data) {
            if (data.errcode === 200) {
                this.$toast.center('我们将尽快与您联系，谢谢')
                this.$router.go(-2)
            } else {
                this.$toast.bottom(data.errmsg)
            }
        },
        selectCalendar: function (params) {
            let needChangeTime = this.selcetTime;
            this.$router.push({ name: 'cladr', query: { slCla: needChangeTime + ',' + needChangeTime, single: true } })
        },
        jumpDown() {
            window.scrollTo(0, 2000)
        },
        getToCrv(){
            this.$router.push({path:'crv'})
        }
    },
    watch: {
        Upinfo: {
            handler: function (newVal) {
                this.$store.commit('saveCustomizationinfo', this.Upinfo)
            },
            deep: true
        }
    },
    mounted() {
        let _this = this;
        window.addEventListener('scroll', function () {
            window.scrollY > 700 ? _this.dpButtom = false : _this.dpButtom = true;
        });
    }
}
</script>

<style>
body{
    background-color: #ffffff;
}
.dz_down_button {
    position: fixed;
    right: 16px;
    bottom: 16px;
    width: 40px;
    z-index: 3
}

#tamMain,
#titleImg {
    width: 100%;
    float: left;
}

#manTia {
    color: #ffffff;
    width: 76%;
    padding: 12%;
    position: absolute;
    left: 0;
    top: 0;
}

#manTia div {
    width: 100%;
    text-align: center;
    font-size: 40px;
    font-weight: bold;
}

#manTia_Tschild {
    position: absolute;
    color: #ffffff;
    left: 50%;
    top: 0;
    margin: 34.5% -140px;
    font-size: 16px;
    width: 280px;
    /*text-indent: 2em;*/
    line-height: 1.8em;
    letter-spacing: 2px;
    word-spacing: 2px;
}

.mass_info {
    padding: 3.3%;
    float: left;
    width: 93.4%;
    background-color: #ffffff;
}

.mess_img {
    padding: 3.3% 0;
    width: 49%;
}

.mess_Content {
    background-color: #ffffff;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: space-between;
    -webkit-justify-content: space-between;
    padding: 3.3%;
    float: left;
    width: 93.4%;
}

.mess_info {
    padding: 0;
    width: 48%;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    color: #666666;
    font-size: 14px;
}

.mess_info div:first-child {
    padding: 7% 0;
    width: 100%;
    text-align: center;
    font-weight: bold;
}

.mess_info div:last-child {
    line-height: 1.3em;
    letter-spacing: 2px;
    word-spacing: 2px;
}

.mass_title {
    padding: 3.3% 0;
    float: left;
    text-align: center;
    width: 100%;
    color: #666666;
    font-size: 30px;
}

.mass_titles {
    background-color: #ffffff;
    padding: 7% 0 3.3% 0;
    float: left;
    text-align: center;
    width: 100%;
    color: #666666;
    font-size: 30px;
}

.mass_text {
    padding: 3.3% 0;
    float: left;
    width: 100%;
    color: #666666;
    font-size: 16px;
    line-height: 1.3em;
    letter-spacing: 2px;
    word-spacing: 2px;
}

.mass_img {
    padding: 3.3% 0;
    float: left;
    width: 100%;
}

.mass_more {
    padding: 0 0 3.3% 0;
    color: #333333;
    font-size: 12px;
    float: left;
    width: 100%;
    text-align: right;
}

.mass_inputLine {
    padding: 3.3% 0;
    float: left;
    width: 100%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    border-bottom: 1px solid #f5f5f5;
}

.mass_inputLine input {
    font-size: 16px;
    color: #666666;
    border: 0;
    padding: 0;
    line-height: 30px;
    outline: none;
    flex: 1;
    -webkit-flex: 1;
}

.change_mass {
    flex: 1;
    -webkit-flex: 1;
    font-size: 16px;
    color: #666666;
    text-align: right;
}

.mass_inputLine img {
    margin-left: 3.3%;
    margin-top: 2px;
    height: 16px;
}

.inputTitle {
    width: 100px;
}

.mass_inputLine_red {
    padding: 3.3% 0;
    float: left;
    width: 100%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    color: #ff3f5c;
}

.mass_inputLine_red img {
    margin-left: 3.3%;
    margin-top: 2px;
    height: 16px;
}

.mass_bottom_red {
    margin-top: 30%;
    float: left;
    width: 100%;
    text-align: center;
    height: 46px;
    line-height: 46px;
    background-color: #ff3f5c;
    color: #ffffff;
}

.mass_button {
    padding: 5px 10%;
    text-align: center;
    border-bottom: 1px solid #ffffff;
}

.mass_button_select span {
    font-size: 12px;
    color: #999999;
}

.mass_button_select {
    padding: 5px 0;
    flex: 1;
    -webkit-flex: 1;
    text-align: center;
    color: #ff3f5c;
    border-bottom: 1px solid #ff3f5c;
    -webkit-animation: asLong 0.3s
}

@-webkit-keyframes asLong {
    from {
        flex: 0.5;
        -webkit-flex: 0.5;
    }
    to {
        flex: 1;
        -webkit-flex: 1;
    }
}
</style>
