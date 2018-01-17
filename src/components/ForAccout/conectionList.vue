<template>
    <div class="concetNaer">
        <div class="selectArea_conection" v-show="this.conSas.length > 0">
            <div class="linerbar" v-bind:key="'lc'+index" v-for="(item, index) in conSas">
                <div :class="seletBar(index)" @click="useThisItem(item)">
                    <div style="display:flex;justify-content:space-between;">
                        <div>{{item.name}}</div> {{item.phone}}
                    </div>
                    <div>
                        {{item.email}}
                    </div>
                    <div v-if="false">
                       上海市浦东新区喜马拉雅芳甸路1188弄1-4号证大喜马拉雅中心，优客工厂RB105室
                    </div>
                </div>
                <div class="function_area">
                    <div class="selectThis">
                        <img v-show="index==0" src="../../../static/obj_img/copy_img/buy_contacts_icon_1.png">
                        <img v-show="index!=0" src="../../../static/obj_img/copy_img/buy_contacts_icon_2.png">
                        <div :class="selectClass(index)">
                            {{selectText(index)}}
                        </div>
                    </div>
                    <div class="cd_area" @click="editConection(item)">
                        <img src="../../../static/obj_img/copy_img/buy_contacts_edit_icon.png">
                        <div class="c666666">
                            编辑
                        </div>
                    </div>
                    <div class="cd_area" @click="deletItem(item.id)">
                        <img src="../../../static/obj_img/copy_img/buy_contacts_del_icon.png">
                        <div class="c666666">
                            删除
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="add_newinfo" v-show="this.conSas.length > 0" @click="addnewConection">
            <img src="../../../static/obj_img/accout_img/my_linkman_plus.png">
            <div>添加新联系人</div>
        </div>
        <div class="emp_add" v-show="this.conSas.length == 0">
            <img class="add_img_title" src="../../../static/obj_img/accout_img/my_linkman_pic.png">
            <div class="add_text">当前没有联系人信息</div>
            <div class="add_newinfo_2" @click="addnewConection">
                <img src="../../../static/obj_img/accout_img/my_linkman_plus.png">
                <div>添加新联系人</div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    created() {
        this.getUserIndexContact()
    },
    data() {
        return {
            conSas:'',
            selectClass(statue) {
                return statue === 0 ? 'cff3f5c' : 'c666666'
            },
            selectText(statue) {
                return statue === 0 ? '当前订单联系人' : '设为订单联系人'
            },
            seletBar(statue) {
                return statue === 0 ? 'info_area bk_on' : 'info_area bk_normal'
            },
        }
    },
    methods: {
        deletItem(indexID) {
            if (confirm('是否删除该条目')) {
                let a = {
                    id: indexID,
                    access_token: this.$store.state.usrInfomation.access_token
                }
                this.$store.commit('ConectionPara', a)
                let getString = this.$store.state.getParamString;
                let postString = getString.replace('?', '');
                this.$http.post(this.$store.state.serverHost + '/yqhbsp/userinfo/deleteContact', postString).then(m => this.deleteCallback(m.data)).catch(r => this.$toast.bottom('系统错误'));
            }
        },
        useThisItem(item){
            this.$route.query.choose?this.$store.commit('saveConnectionObj',item):''
            this.$router.go(-1)
        },
        deleteCallback(data){
            if (data.errcode === 200) {
                this.getUserIndexContact()
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        addnewConection: function () {
            this.$router.push({ path: '/acv' })
        },
        editConection: function (item) {
            this.$router.push({ name: 'etCo', query: { coInfo: JSON.stringify(item) } })
        },
        getUserIndexContact() {
            let g = {
                access_token: this.$store.state.usrInfomation.access_token
            }
            this.$store.commit('ConectionPara', g)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/userinfo/indexContact' + getString).then(m => this.setUserIndexContact(m.data)).catch(r => this.$toast.bottom('系统错误'));
        },
        setUserIndexContact(data) {
            if (data.errcode === 200) {
                this.conSas = data.records
            } else {
                this.$toast.center(data.errmsg)
            }
        }
    }
}
</script>

<style>
body {
    background-color: #f5f5f5
}

.concetNaer {
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    flex-direction: column;
    -webkit-flex-direction: column;
    width: 100%;
    min-height: 100vh;
}

.emp_add {
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    flex-direction: column;
    -webkit-flex-direction: column;
    width: 100%;
    min-height: 100vh;
}

.selectArea_conection {
    width: 100%;
    flex: 1;
    -webkit-flex: 1;
    float: left;
}

.info_area {
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    padding: 3.4% 3.8%;
    margin-bottom: 1px;
}

.bk_on {
    background-color: #ff3f5c;
    color: #ffffff;
}

.bk_normal {
    background-color: #ffffff;
    color: #333333;
}

.info_area div {
    line-height: 20px;
    font-size: 14px;
    text-align: justify
}
.info_area div:nth-child(2){
    margin:10px 0;
}
.function_area {
    display: flex;
    display: -webkit-flex;
    background-color: #ffffff;
    padding: 3.4% 3.8%;
    margin-bottom: 1.4%;
    align-items: center;
    -webkit-align-items: center;
}

.function_area img {
    margin: 0 4px 0 0;
    height: 20px;
}

.selectThis {
    display: flex;
    display: -webkit-flex;
    flex: 1;
    -webkit-flex: 1;
    float: left;
    align-items: center;
    -webkit-align-items: center;
}

.cd_area {
    display: flex;
    display: -webkit-flex;
    font-size: 14px;
    color: #666666;
    margin-left: 3%;
    align-items: center;
    -webkit-align-items: center;
}

.cff3f5c {
    font-size: 14px;
    color: #ff3f5c;
}

.c666666 {
    font-size: 14px;
    color: #666666;
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
    font-size: 14px;
    font-weight: bold;
}

.add_newinfo img {
    height: 20px;
    margin-right: 2.7%;
}

.add_img_title {
    width: 24.2%;
    margin-top: 56%;
}

.add_text {
    margin: 2% 0 7.2% 0;
    width: 70%;
    text-align: center;
    color: #666666;
}

.add_newinfo_2 {
    float: left;
    width: 60%;
    background-color: #ff3f5c;
    color: #ffffff;
    display: flex;
    display: -webkit-flex;
    padding: 3.3% 0;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
    font-size: 14px;
    font-weight: bold;
    -webkit-border-radius: 5px;
    border-radius: 5px;
}

.add_newinfo_2 img {
    height: 20px;
    margin-right: 2.7%;
}
</style>
