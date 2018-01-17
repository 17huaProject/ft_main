<template>
    <div class="detail_info">
        <img class="Like_cover_detail" v-show="likeId==''" v-if="this.$store.state.openID!=''" @click="conectionBYid(true)" src="../../static/obj_img/list_Collect_icon_.png">
        <img class="Like_cover_detail" v-show="likeId!=''" @click="conectionBYid(false)" src="../../static/obj_img/list_Collected_icon.png">
        <div class="img_Title">
            <img :src="this.cheakImgaddr(acti.big_img)">
        </div>
        <div class="Tib_bar_detail" v-show="false">
            <img src="../../static/obj_img/list_img/list_time_icon.png">
            <div>{{eventTime}}</div>
        </div>
        <div class="MainTitle_bar">
            <div class="paintion_title">{{acti.event_name}}</div>
            <div class="level_Area">
                <div>难度</div>
                <img class="star_af" :src="DrawLevel(acti.easy_level,0)">
                <img class="star_af" :src="DrawLevel(acti.easy_level,1)">
                <img class="star_af" :src="DrawLevel(acti.easy_level,2)">
                <img class="star_af" :src="DrawLevel(acti.easy_level,3)">
                <img class="star_af" :src="DrawLevel(acti.easy_level,4)">
            </div>
            <div class="end_anonce" v-show="acti.event_status!='FINISH'">活动时间</div>
            <div class="end_anonce" v-show="acti.event_status=='FINISH'">&nbsp;</div>
            <div class="charge_">
                <div class="Money_" v-if="acti.event_status!='SOLDOUT'">
                    ￥{{acti.price}}
                </div>
                <div class="Money_" style="font-size:18px;margin-top:5px" v-else>
                    已售完
                </div>
                <div class="Join_" v-if="acti.event_status=='SOLDOUT'">
                    下次早点来哦
                </div>
                <div class="Join_" v-else-if="acti.sold>5&&acti.event_status!='SOLDOUT'">
                    已有{{acti.sold}}人参加
                </div>
                <div class="Join_" v-else>
                    已有小伙伴参加
                </div>
                <div class="count_down_box_acd" v-show="false">
                    {{closeTime.d}}天
                    <div>{{closeTime.h}}</div>小时
                    <div>{{closeTime.m}}</div>分
                </div>
                <div class="count_down_box_acd" v-show="acti.event_status!='FINISH'">
                    <div>{{activeTime.m}}</div>月
                    <div>{{activeTime.d}}</div>日
                    <span>{{activeTime.h}}</span>
                </div>
                <div class="count_down_box_acd" v-show="acti.event_status=='FINISH'">
                    <span style="font-size:16px">活动已结束</span>
                </div>
            </div>
        </div>
        <div class="Free_sent_dl" v-show="acti.is_refund==1||acti.free_service!=''">
            <div class="Free_Pade_del" v-show="acti.is_refund==1">
                <img src="../../static/obj_img/acti_img/act_tui_icon.png">48小时退
            </div>
            <div class="Free_Pade_del" v-show="this.ServiceOnFree('SNACK')">
                <img src="../../static/obj_img/acti_img/act_food_icon.png">精美零食
            </div>
            <div class="Free_Pade_del" v-show="this.ServiceOnFree('DRINK')">
                <img src="../../static/obj_img/acti_img/act_wine_icon.png">饮料酒水
            </div>
            <div class="Free_Pade_del" v-show="this.ServiceOnFree('WIFI')">
                <img src="../../static/obj_img/acti_img/activity_wifi_icon.png">免费WiFi
            </div>
        </div>
        <div class="Active_address_" @click="GoADDetail">
            <div>{{acti.venue_name}}</div>
            <img src="../../static/obj_img/list_icon_arrRight.png">
        </div>
        <div class="Active_address_" @click="jumpToamap">
            <div>{{acti.address}}</div>
            <img src="../../static/obj_img/list_icon_arrRight.png">
        </div>
        <div class="Member_Join" v-show="joinMenber.length>10">
            <!-- @click="getMenberList"  -->
            <div class="jo_text">以下小伙伴，已参加...</div>
            <div class="jo_img">
                <img :key="item.nickname+index" v-for="(item,index) in joinMenber" :src="item.avatar" v-show="index<5">
            </div>
        </div>
        <div class="Content_info_actv" style="padding: 2.1% 2.9% 6.3% 2.9%">
            <div class="Content_title">
                <b>·</b>活动详情
                <b>·</b>
            </div>
            <div class="Content_info_box" v-html="this.unescape(acti.event_content)">

            </div>
        </div>
        <div class="Content_info_actv" v-show="false">
            <div class="Content_title">
                <b>·</b>作画步骤
                <b>·</b>
            </div>
            <div class="Content_info_box" v-html="this.unescape(acti.art_content)">
            </div>
        </div>
        <div class="Content_info_actv">
            <div class="Content_title">
                <b>·</b>关于画师
                <b>·</b>
            </div>
            <div class="Content_info_box_P">
                <img class="artist_title" :src="this.cheakImgaddr(acti.artist_avatar)">
                <div class="Content_Painter_right">
                    <div class="c_text_panter">{{acti.artist_desc}}</div>
                    <div class="level_panter">
                        <div>评级：</div>
                        <img :src="PainterLevel(acti.artist_level,0)">
                        <img :src="PainterLevel(acti.artist_level,1)">
                        <img :src="PainterLevel(acti.artist_level,2)">
                        <img :src="PainterLevel(acti.artist_level,3)">
                        <img :src="PainterLevel(acti.artist_level,4)">
                    </div>
                    <div class="comPass_painter">
                        <div>
                            <img src="../../static/obj_img/acti_img/act_painter_icon_1.png">身份已验证</div>
                        <div>
                            <img src="../../static/obj_img/acti_img/act_painter_icon_2.png">画师资格已验证</div>
                    </div>
                </div>
            </div>
        </div>
        <div class="findDetailThis" @click="GoADetail()">详细介绍</div>
        <div class="Content_info_actv">
            <div class="Content_title">
                <b>·</b>地点介绍
                <b>·</b>
            </div>
            <img class="Content_venue_img" :src="this.cheakImgaddr(acti.venue_img)">
            <div class="Content_info_box" v-html="this.unescape(acti.venue_content)">
            </div>
        </div>
        <div class="findDetailThis" @click="GoADDetail()">详细介绍</div>
        <div class="Introduce_Like">
            <div class="May_Like">你应该会喜欢</div>
            <div class="May_List">
                <areabox v-show="listArr[index].id != acti.id" :key="listArr[index].id" v-for="(item, index) in listArr" :message='listArr[index]'></areabox>
            </div>
        </div>
        <div class="Pay_bottom" v-if="acti.event_status=='ONSALE'">
            <div class="Buy_botton" @click="CreatOrder" v-show="!Dealing">买单</div>
            <div class="Buy_botton_w" v-show="Dealing">买单</div>
            <!-- <div class="triangle-topright"></div> -->
            <div class="Charge_info">
                <div>￥{{acti.price}}</div>
                <div v-show="false">还剩{{acti.left_num}}名资格</div>
            </div>
        </div>
        <div class="Pay_bottom" v-else-if="acti.event_status=='PRESALE'">
            <div class="Buy_botton" @click="CreatOrder" v-show="!Dealing">预售</div>
            <div class="Buy_botton_w" v-show="Dealing">预售</div>
            <!-- <div class="triangle-topright"></div> -->
            <div class="Charge_info">
                <div>￥{{acti.price}}</div>
                <div>还剩{{acti.left_num}}名资格</div>
            </div>
        </div>
        <!-- <div class="Pay_bottom" v-show="acti.event_status=='SOLDOUT'">
                                <div class="Buy_botton_w">已售空</div>
                                <div class="triangle-topright"></div>
                                <div class="Charge_info">
                                    <div>￥{{acti.price}}</div>
                                    <div>售罄</div>
                                </div>
                            </div>
                            <div class="Pay_bottom" v-show="acti.event_status=='FINISH'">
                                <div class="Buy_botton_w">活动结束</div>
                                <div class="triangle-topright"></div>
                                <div class="Charge_info">
                                    <div>￥{{acti.price}}</div>
                                    <div> </div>
                                </div>
                            </div> -->
    </div>
</template>

<script>
import ActiveBox from './ForList/ListBarStyle_1.vue'
const weekName = ['日', '一', '二', '三', '四', '五', '六'];
export default {
    data() {
        return {
            Like: true,
            acti: {},
            Dealing: false,
            activeID: {
                id: this.changeIdMsu(this.$route.query.actinfo) || 0,
                access_token: this.$store.state.usrInfomation.access_token || ''
            },
            likeId: '',
            closeTime: {
                d: '',
                h: '',
                m: '',
            },
            acService: '',
            activeTime: {
                m: '',
                d: '',
                h: '',
            },
            evContent: '',
            eventTime: '',
            JoinNum: 20,
            CD_day: 2,
            CD_hour: '06',
            CD_mini: 14,
            listArr: [],
            joinMenber: [],
        }
    },
    created() {
        this.getDetailMaster()
         window.scrollTo(0, 0)
    },
    methods: {
        LikeThisActivity(data) {
            if (data.errcode == 200 && this.likeId == '') {
                this.likeId = data.record.id;
                this.$toast.bottom('收藏成功')
            } else if (data.errcode == 200 && this.likeId != '') {
                this.likeId = ''
                this.$toast.bottom('取消收藏')
            } else {
                this.$toast.bottom(data.errmsg)
            }
        },
        cheakImgaddr(add) {
            add = this.$store.state.PROTOCOL?add.replace(/\http:/g,'https:'):add
            try {
                return (add.indexOf('http') < 0 ? this.$store.state.SyetemBaseConfig.image_prefix + add : '' + add)
            } catch (error) {
                return ''
            }
        },
        ServiceOnFree(data) {
            try {
                return this.acService.indexOf(data) >= 0
            } catch (error) {

            }
        },
        changeIdMsu(data) {
            return parseInt(data)
        },
        CreatOrder() {
            this.Dealing = true;
            let comInfo = {
                Event_id: this.changeIdMsu(this.$route.query.actinfo),
                OrderInfo: this.acti,
            }
            comInfo.OrderInfo.img_title = this.acti.big_img.indexOf('http') < 0 ? this.$store.state.SyetemBaseConfig.image_prefix + this.acti.big_img : '' + this.acti.big_img.replace(/\http:/g,'');
            this.$store.commit('setCommodityInfo', comInfo)
            this.$store.commit('saveCouponInfo', {})
            this.$store.commit('savebuyNumber', 1)
            this.$router.push({ path: '/pa' })
        },
        getMenberList() {
            this.$router.push({ name: 'jona', query: { actinfo: this.$route.query.actinfo } })
        },
        GoADetail() {
            this.$router.push({ name: 'ptinfo', query: { prtinfo: this.acti.artist_id } })
            // this.$router.push({ path: '/pt' })
        },
        GoADDetail() {
            this.$router.push({ name: 'atinfo', query: { addinfo: this.acti.venue_id } })
        },
        getJoinMember() {
            let J = {
                id: this.changeIdMsu(this.$route.query.actinfo)
            }
            this.$store.commit('ConectionPara', J)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/event/showPaidUser' + getString).then(m => this.setJoinMenber(m.data)).catch(r => console.log(r));
        },
        setJoinMenber(data) {
            if (data.errcode === 200) {
                this.joinMenber = data.records
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        getDetailMaster() {
            this.$store.commit('ConectionPara', this.activeID)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/event/show' + getString).then(m => this.setActiveDetail(m.data)).catch(r => console.log(r));
        },
        setActiveDetail(data) {
            if (data.errcode === 200) {
                this.acti = data.record.event
                data.record.is_faverate == 1 ? this.likeId = data.record.faverate.id : '';
                this.acService = data.record.event.free_service;
                this.evContent = data.record.event.event_content;
                this.congGsTime(data.record.event.event_time.replace(/\-/g, '/'))
                this.eventTimeShow(data.record.event.event_time.replace(/\-/g, '/'))
                let share = {
                    sharetitle: '我喜欢<' + this.acti.event_name + '>绘画活动，一起画吧！',
                    sharelink: 'https://wx.17hua.me/#/dt?actinfo=' + this.$route.query.actinfo+'&u_id='+(this.$store.state.usrInfomation.user_id||17081707011),
                    shareimgUrl: this.acti.big_img.indexOf('http') < 0 ? this.$store.state.SyetemBaseConfig.image_prefix + this.acti.big_img : '' + this.acti.big_img,
                    shareDesc: '我推荐一起画，你不来么？'
                }
                this.$store.commit('useShareFun', share)

            } else {
                this.$toast.center(data.errmsg)
            }
            this.getJoinMember();
        },
        unescape(html) {
            html = this.$store.state.PROTOCOL?html.replace(/\http:/g,'https:'):html
            try {
                return html
                    .replace(html ? /&(?!#?\w+;)/g : /&/g, '&amp;')
                    .replace(/&lt;/g, "<")
                    .replace(/&gt;/g, ">")
                    .replace(/&quot;/g, "\"")
                    .replace(/&#39;/g, "\'")
                    .replace(/&mdash;/g, "-")
                    .replace(/&nbsp;/g, " ")
                    .replace(/&ldquo;/g, "“")
                    .replace(/&rdquo;/g, "”")
            } catch (error) {

            }
        },
        DrawLevel(num, ad) {
            if (num - ad >= 1) {
                return '../../static/obj_img/acti_img/activity_star_icon_1.png'
            } else if (num - ad > 0 && num - ad < 1) {
                return '../../static/obj_img/acti_img/activity_star_icon_2.png'
            } else {
                return '../../static/obj_img/acti_img/activity_star_icon_3.png'
            }
        },
        PainterLevel(num, ad) {
            if (num - ad >= 1) {
                return '../../static/obj_img/list_img/list_Star_icon_1.png'
            } else if (num - ad > 0 && num - ad < 1) {
                return '../../static/obj_img/list_img/list_Star_icon_2.png'
            } else {
                return '../../static/obj_img/list_img/list_Star_icon_3.png'
            }
        },
        congGsTime(data) {
            let nowTime = new Date().getTime();
            let closeTime = new Date(data).getTime();
            let actTime = new Date(data);
            let chag = new Date(closeTime - nowTime).getTime() / 1000;
            if (chag > 0) {
                this.closeTime.d = Math.floor(chag / (24 * 60 * 60));
                this.closeTime.h = Math.floor(chag / (60 * 60) % 24) < 10 ? '0' + Math.floor(chag / (60 * 60) % 24) : Math.floor(chag / (60 * 60) % 24);
                this.closeTime.m = Math.floor(chag / 60 % 60) < 10 ? '0' + Math.floor(chag / 60 % 60) : Math.floor(chag / 60 % 60);
                let s = Math.floor(chag % 60);
                this.runTime(data, s);
            } else {
                this.closeTime.d = 0;
                this.closeTime.h = 0;
                this.closeTime.m = 0;
            }
            this.activeTime.m = 1 + actTime.getMonth();
            this.activeTime.d = actTime.getDate();
            //this.activeTime.h = actTime.getHours() > 12 ? '下午' + (actTime.getHours() - 12) + ':' + (actTime.getMinutes() < 10 ? '0' + actTime.getMinutes() : actTime.getMinutes()) : '上午' + actTime.getHours() + ':' + (actTime.getMinutes() < 10 ? '0' + actTime.getMinutes() : actTime.getMinutes());
            this.activeTime.h = actTime.getHours() > 10 ? actTime.getHours() + ':' + (actTime.getMinutes() < 10 ? '0' + actTime.getMinutes() : actTime.getMinutes()) : '0' + actTime.getHours() + ':' + (actTime.getMinutes() < 10 ? '0' + actTime.getMinutes() : actTime.getMinutes());
        },
        eventTimeShow(data) {
            let eventTime = new Date(data);
            this.eventTime += (eventTime.getMonth() + 1) < 10 ? '0' + (eventTime.getMonth() + 1) : eventTime.getMonth() + 1;
            this.eventTime += '月'
            this.eventTime += eventTime.getDate() < 10 ? '0' + eventTime.getDate() : eventTime.getDate();
            this.eventTime += '日 '
            this.eventTime += '周' + weekName[eventTime.getDay()] + ' '
            this.eventTime += eventTime.getHours() < 10 ? '0' + eventTime.getHours() : eventTime.getHours();
            this.eventTime += ':'
            this.eventTime += eventTime.getMinutes() < 10 ? '0' + eventTime.getMinutes() : eventTime.getMinutes();
        },
        runTime(data, s) {
            var self = this
            s = s || 60;
            setTimeout(function() {
                self.congGsTime(data)
            }, s * 1000);
        },
        jumpToamap() {
            this.$router.push({ name: 'amapif', query: { lginfo: this.acti.position, lgname: this.acti.address } })
        },
        conectionBYid(statue) {
            if (statue) {
                let thisCC = {
                    type: 'EVENT',
                    related_id: this.$route.query.actinfo,
                    access_token: this.$store.state.usrInfomation.access_token
                }
                this.$store.commit('ConectionPara', thisCC)
                let getString = this.$store.state.getParamString;
                let postString = getString.replace('?', '');
                this.$http.get(this.$store.state.serverHost + '/yqhbsp/faverate/add' + getString).then(m => this.LikeThisActivity(m.data)).catch(r => this.$toast.bottom(r));
            } else {
                let thisCC = {
                    type: 'EVENT',
                    id: this.likeId,
                    access_token: this.$store.state.usrInfomation.access_token
                }
                this.$store.commit('ConectionPara', thisCC)
                let getString = this.$store.state.getParamString;
                let postString = getString.replace('?', '');
                this.$http.get(this.$store.state.serverHost + '/yqhbsp/faverate/delete' + getString).then(m => this.LikeThisActivity(m.data)).catch(r => this.$toast.bottom(r));
            }
        },
        getListFot() {
            let ListConfig = {
                city_code: this.$store.state.SelectCityCode,
                p: '',
                d: 500,
                stext: '',
                start: this.YYYYMMdd(new Date()),
                end: this.YYYYMMdd(new Date(new Date().getTime() + parseInt(7 * 24 * 60 * 60 * 1000))),
                num: 1,
                page_no: 1,
                page_size: 5,
                sort: 'closing_time|0'
            }
            this.$store.commit('ConectionPara', ListConfig)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/event/search' + getString).then(m => this.setDialogList_a(m.data)).catch(r => console.log(r));
        },
        YYYYMMdd(data) {
            let ed = new Date(data);
            var yyyy = ed.getFullYear().toString();
            var MM = this.pad(ed.getMonth() + 1, 2);
            var dd = this.pad(ed.getDate(), 2);
            return yyyy + '-' + MM + '-' + dd;
        },
        setDialogList_a(data) {
            if (data.errcode === 200) {
                this.listArr = data.records
            } else {
                this.$toast.bottom(data.errmsg)
            }
        },
        pad(number, length) {
            var str = '' + number;
            while (str.length < length) {
                str = '0' + str;
            }
            return str;
        },
    },
    watch: {

    },
    components: {
        areabox: ActiveBox,
    },
    mounted() {
        this.getListFot()
        let share = {
            sharetitle: '我喜欢<' + this.acti.event_name + '>绘画活动，一起画吧！',
            sharelink: 'wx.17hua.me/#/dt?actinfo=' + this.$route.query.actinfo+'&u_id=' + this.$store.state.usrInfomation.user_id,
            shareimgUrl: this.acti.big_img.indexOf('http') < 0 ? this.$store.state.SyetemBaseConfig.image_prefix + this.acti.big_img : '' + this.acti.big_img,
            shareDesc: '我推荐一起画，你不来么？'
        }
        this.$store.commit('useShareFun', share)
        // if (sessionStorage.getItem('CreateOrder')) {
        //     sessionStorage.removeItem('CreateOrder')
        //     this.CreatOrder();
        // }
    }
}


</script>

<style>
body {
    background-color: #f5f5f5
}

.Pay_bottom {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 60px;
}

.detail_info,
.img_Title,
.charge_,
.Free_sent_dl,
.Introduce_Like {
    float: left;
    width: 100%;
}

.Introduce_Like {
    padding-bottom: 60px;
}

.img_Title,
.charge_,
.MainTitle_bar,
.Free_sent_dl,
.Active_address_,
.Member_Join,
.Content_info_actv {
    background-color: #ffffff;
}

.charge_,
.Free_sent_dl,
.Free_Pade_del,
.Active_address_,
.Member_Join {
    display: flex;
    display: -webkit-flex;
}

.MainTitle_bar {
    float: left;
    width: 94.2%;
    padding: 0 2.9% 2.1% 2.9%;
    margin-bottom: 1%;
}



.Member_Join {
    float: left;
    width: 94.2%;
    padding: 2.1% 2.9%;
    margin-bottom: 1.4%;
}

.Active_address_ {
    float: left;
    width: 94.2%;
    padding: 0 2.9%;
    margin-bottom: 1%;
}

.Free_sent_dl {
    float: left;
    width: 94.2%;
    padding: 0 2.9%;
    margin-bottom: 1%;
}

.Content_info_actv {
    float: left;
    width: 94.2%;
    padding: 2.1% 2.9% 10% 2.9%;
    border-bottom: 1px solid #f2f2f2;
}

.img_Title img {
    float: left;
    width: 100%;
}

.Tib_bar_detail {
    float: left;
    width: 92%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    font-size: 12px;
    color: #ffffff;
    height: 40px;
    margin-top: -40px;
    padding: 0 4%
}

.Tib_bar_detail img {
    width: 3.3%;
    margin-right: 1.1%
}

.Tib_bar_detail div {
    margin-top: -1px;
    margin-right: 3.2%
}


























/*标题栏*/

.Like_cover_detail {
    position: absolute;
    right: 0;
    top: 0;
    width: 7.2%;
    margin: 3.8% 3.2%;
}

.paintion_title {
    font-weight: bold;
    padding-top: 2.7%;
    font-size: 18px;
    color: #333333;
}

.level_Area {
    display: flex;
    display: -webkit-flex;
    color: #ff3300;
    font-weight: bold;
    line-height: 15px;
    padding-top: 3%;
    font-size: 14px;
    align-items: flex-end;
    -webkit-align-items: flex-end;
}

.level_Area div {
    display: flex;
    display: -webkit-flex;
    color: #333333;
    font-weight: normal;
    line-height: 12px;
    margin-right: 5px
}

.level_Area img {
    width: 16px;
    height: 16px;
    margin: -1px 4px 0 0;
}

.end_anonce {
    width: 100%;
    text-align: right;
    font-size: 12px;
    margin-top: -5px;
    color: #666666
}

.Money_ {
    font-size: 24px;
    color: #ff3f5c;
    font-weight: bold;
}

.Join_ {
    font-size: 12px;
    color: #666666;
    margin-left: 2%;
    line-height: 38px
}

.count_down_box_acd {
    display: flex;
    display: -webkit-flex;
    flex: 1;
    -webkit-flex: 1;
    justify-content: flex-end;
    -webkit-justify-content: flex-end;
    line-height: 38px;
    font-size: 14px;
    color: #666666;
    align-items: center;
    -webkit-align-items: center;
}

.count_down_box_acd div {
    width: 24px;
    height: 24px;
    line-height: 24px;
    text-align: center;
    background-color: #ff3f5c;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    margin: 0 5px;
    font-size: 14px;
    color: #ffffff
}

.count_down_box_acd span {
    height: 24px;
    line-height: 24px;
    text-align: center;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    margin: 0 0 0 5px;
    font-size: 14px;
    color: #ff3f5c
}


























/*免费服务*/

.Free_Pade_del {
    padding: 2.9% 8% 2.9% 0;
    float: left;
    display: flex;
    display: -webkit-flex;
    font-size: 12px;
    line-height: 14px;
    height: 14px;
    justify-content: center;
    -webkit-justify-content: center;
    color: #666666;
}

.Free_Pade_del img {
    height: 14px;
    margin-right: 2px;
}


























/*举办地*/

.Active_address_ {
    /* font-weight: bold; */
    color: #333333;
    align-items: center;
    -webkit-align-items: center;
    justify-content: space-between;
    -webkit-justify-content: space-between;
}

.Active_address_ div {
    padding: 2.9% 0;
    font-size: 14px;
    float: left;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

.Active_address_ img {
    float: right;
    height: 14px;
    margin: 0 0 0 5px;
}







































/*参加朋友*/

.Member_Join {
    flex-direction: column;
    -webkit-flex-direction: column;
}

.jo_text {
    float: left;
    flex: 1;
    -webkit-flex: 1;
    font-size: 14px;
    color: #333333
}

.jo_img {
    margin: 2.7% 0;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: space-between;
    -webkit-justify-content: space-between;
}

.jo_img img {
    -webkit-border-radius: 100px;
    border-radius: 100px;
    width: 16%;
}







































/*活动内容*/

.Content_title {
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
    flex: 1;
    -webkit-flex: 1;
    font-weight: bold;
    font-size: 18px;
    padding: 3.3% 0 2.7% 0;
}

.Content_title b {
    font-size: 24px;
    margin: 0 10px;
}

.Content_info_box {
    float: left;
    width: 100%;
    /* text-indent: 2em;
    line-height: 1.5em;
    letter-spacing: 2px;
    word-spacing: 4px; */
    color: #666666;
    overflow-x: hidden
}

.Content_info_box div,
.Content_info_box p {
    float: left;
    width: 100%;
    max-width: 100%;
    height: auto;
}

.Content_info_box img {
    float: left;
    width: 100%;
    max-width: 100%;
    height: auto;
    padding: 3.3% 0 2.7% 0;
}

.Content_info_box_P {
    float: left;
    width: 100%;
    overflow-x: hidden;
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
}

.Content_info_box_P img {
    width: 120px;
    height: 120px;
    margin: 0 3.3% 0 0;
    float: left;
    -webkit-border-radius: 60px;
    border-radius: 60px;
}

.Content_Painter_right {
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
}

.c_text_panter {
    flex: 1;
    -webkit-flex: 1;
    text-indent: 2em;
    line-height: 1.5em;
    letter-spacing: 2px;
    word-spacing: 4px;
    font-size: 14px;
    color: #666666;
}

.level_panter {
    margin-top: 10px;
    width: 100%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    font-size: 14px;
    color: #666666;
}

.level_panter img {
    float: left;
    width: 14px;
    height: 14px;
    margin: 0 5px 0 0;
}

.comPass_painter {
    margin-top: 10px;
    width: 100%;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    font-size: 12px;
    color: #666666;
}

.comPass_painter div {
    float: left;
    padding-bottom: 5px;
}

.comPass_painter img {
    float: left;
    width: 14px;
    height: 14px;
}































/*列表内容*/

.May_Like {
    width: 100%;
    text-align: center;
    font-weight: bold;
    font-size: 18px;
    margin: 3.3% 0 0 0;
}

.May_list {
    width: 100%;
    float: left;
}





































/*购买按钮部分*/

.Buy_botton {
    position: absolute;
    right: 0;
    bottom: 0;
    width: 67.6%;
    text-align: center;
    background-color: #ff3f5c;
    color: #ffffff;
    font-size: 18px;
    height: 18px;
    line-height: 18px;
    font-weight: bold;
    padding: 16px 0;
    z-index: 10;
}

.Buy_botton_w {
    position: absolute;
    right: 0;
    bottom: 0;
    width: 67.6%;
    text-align: center;
    background-color: #999999;
    color: #ffffff;
    font-size: 18px;
     height: 18px;
    line-height: 18px;
    font-weight: bold;
    padding: 16px 0;
    z-index: 10;
}

.triangle-topright {
    width: 0;
    height: 0;
    border-top: 41px solid rgba(200, 0, 0, 0.5);
    border-left: 4px solid transparent;
    position: absolute;
    right: 0;
    bottom: 0;
    margin-right: 67.6%;
    z-index: 9
}

.Charge_info {
    position: absolute;
    left: 0;
    bottom: 0;
    margin-right: 67.6%;
    width: 33.4%;
    background-color: rgba(255, 255, 255, 0.9);
    height: 50px;
    z-index: 8;
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    -webkit-justify-content: center;
    flex-direction: column;
    -webkit-flex-direction: column;
}

.Charge_info div:first-child {
    text-align: center;
    font-size: 22px;
    color: #ff3f5c;
    font-weight: bold;
    line-height: 22px;
}

.Charge_info div:last-child {
    text-align: center;
    font-size: 12px;
    color: #999999;
    line-height: 14px;
}

.findDetailThis {
    width: 94.2%;
    padding: 0 2.9%;
    height: 40px;
    line-height: 40px;
    margin-top: -40px;
    float: left;
    text-align: right;
    font-size: 14px;
    color: #ff3f5c;
    text-decoration: underline
}

.May_List {
    width: 100%;
    margin-top: -6.5%;
    margin-bottom: 60px;
}

.Content_venue_img {
    width: 100%;
    margin-bottom: 5%;
}
</style>
