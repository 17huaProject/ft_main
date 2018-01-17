<template>
    <div class="detail_info">
        <img class="Like_cover_3" v-show="likeId==''" @click="conectionBYid(true)" src="../../static/obj_img/list_Collect_icon_.png">
        <img class="Like_cover_3" v-show="likeId!=''" @click="conectionBYid(false)" src="../../static/obj_img/list_Collected_icon.png">
        <div class="img_Title">
            <img :src="painter.big_img.indexOf('http')<0?this.$store.state.SyetemBaseConfig.image_prefix+painter.big_img:''+painter.big_img.replace(/\http:/g,'')">
        </div>
        <div class="Tib_bar_p">
            <img src="../../static/obj_img/pent_img/paiter_shengfen_icon.png">
            <div>身份已验证</div>
            <img src="../../static/obj_img/pent_img/paiter_rengzheng_icon.png">
            <div>画师资格已验证</div>
        </div>
        <div class="MainTitle_bar">
            <div class="paintion_title">{{painter.artist_name}}</div>
            <div class="level_Area">
                <div>评级</div>
                <img class="star_af" :src="painterLevel(painter.artist_level,0)">
                <img class="star_af" :src="painterLevel(painter.artist_level,1)">
                <img class="star_af" :src="painterLevel(painter.artist_level,2)">
                <img class="star_af" :src="painterLevel(painter.artist_level,3)">
                <img class="star_af" :src="painterLevel(painter.artist_level,4)">
            </div>
            <!-- <div class="altic_painter painter_s" v-html="this.unescape(painter.artist_desc)">
                
                        </div> -->
            <div class="altic_painter painter_s" v-html="this.unescape(painter.content)">
    
            </div>
        </div>
        <div class="Introduce_Like">
            <div class="May_Like">该画师即将举办</div>
            <div class="May_List">
                <areabox :key="listArr[index].id" v-for="(item, index) in listArr" :message='listArr[index]'></areabox>
                <!-- <areabox></areabox> -->
            </div>
        </div>
    </div>
</template>

<script>
import ActiveBox from './ForList/ListBarStyle_1.vue'
export default {
    data() {
        return {
            painterID: {
                id: parseInt(this.$route.query.prtinfo) || 0,
                access_token: this.$store.state.usrInfomation.access_token || ''
            },
            listArr: [],
            painter: {},
            likeId: '',
            money: 99,
            JoinNum: 20,
            CD_day: 2,
            CD_hour: '06',
            CD_mini: 14,
            ac_ad: '上海市浦东新区芳甸路333号喜马拉雅中心B1'
        }
    },
    created() {
        this.getPainterDetailMaster();

    },
    methods: {
        LikeThisActivity: function (data) {
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
        getPainterDetailMaster() {
            this.$store.commit('ConectionPara', this.painterID)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/artist/show' + getString).then(m => this.setPanter(m.data)).catch(r => console.log(r));
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
        setPanter(data) {
            if (data.errcode === 200) {
                data.record.is_faverate == 1 ? this.likeId = data.record.faverate.id : '';
                this.painter = data.record.artist
                this.getListFot();
                // let share = {
                //     sharetitle: '我喜欢这个绘画活动，你也去一起画吧',
                //     sharelink: 'wx.17hua.me',
                //     shareimgUrl: 'http://wx.17hua.me/static/obj_img/logo_for_share.png',
                //     shareDesc: '创建独一无二的作品！'
                // }
                // this.$store.commit('useShareFun', share)
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        painterLevel(num, ad) {
            if (num - ad >= 1) {
                return '../../static/obj_img/list_img/list_Star_icon_1.png'
            } else if (num - ad > 0 && num - ad < 1) {
                return '../../static/obj_img/list_img/list_Star_icon_2.png'
            } else {
                return '../../static/obj_img/list_img/list_Star_icon_3.png'
            }
        },
        conectionBYid(statue) {
            if (statue) {
                let thisCC = {
                    type: 'ARTIST',
                    related_id: this.$route.query.prtinfo,
                    access_token: this.$store.state.usrInfomation.access_token
                }
                this.$store.commit('ConectionPara', thisCC)
                let getString = this.$store.state.getParamString;
                let postString = getString.replace('?', '');
                this.$http.get(this.$store.state.serverHost + '/yqhbsp/faverate/add' + getString).then(m => this.LikeThisActivity(m.data)).catch(r => console.log(r));
            } else {
                let thisCC = {
                    type: 'ARTIST',
                    id: this.likeId,
                    access_token: this.$store.state.usrInfomation.access_token
                }
                this.$store.commit('ConectionPara', thisCC)
                let getString = this.$store.state.getParamString;
                let postString = getString.replace('?', '');
                this.$http.get(this.$store.state.serverHost + '/yqhbsp/faverate/delete' + getString).then(m => this.LikeThisActivity(m.data)).catch(r => console.log(r));
            }
        },
        getListFot() {
            let ListConfig = {
                city_code: this.$store.state.SelectCityCode,
                p: this.$store.state.locationIN.latitude + ',' + this.$store.state.locationIN.longitude,
                d: 500,
                stext: this.painter.artist_name,
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
    components: {
        areabox: ActiveBox,
    },
    mounted() {
        // let share = {
        //     sharetitle: this.painter.artist_name,
        //     sharelink: 'wx.17hua.me/pt?prtinfo=' + this.$route.query.prtinfo,
        //     shareimgUrl: this.$store.state.SyetemBaseConfig.image_prefix + this.painter.big_img,
        //     shareDesc: '这个画师很厉害哦，我们跟着一起画吧！'
        // }
        // this.$store.commit('useShareFun', share)
    }
}
</script>

<style>
body {
    background-color: #f5f5f5
}

.altic_painter {
    padding: 3.7% 0;
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
.Free_sent,
.Active_address,
.altic_painter,
.Introduce_Like {
    float: left;
    width: 100%;
}

.Introduce_Like {
    padding-bottom: 5%;
}

.star_af {
    width: 16px;
    height: 16px;
    margin: 8px 4px 0 0
}

.img_Title,
.charge_,
.MainTitle_bar,
.Free_sent,
.Active_address,
.Member_Join,
.Content_info,
.altic_painter {
    background-color: #ffffff;
}

.charge_,
.Free_sent,
.Active_address,
.Member_Join {
    display: flex;
    display: -webkit-flex;
}

.MainTitle_bar {
    float: left;
    width: 94.2%;
    padding: 0 2.9% 2.1% 2.9%;
    margin-bottom: 1.4%;
}



.Member_Join {
    float: left;
    width: 94.2%;
    padding: 2.1% 2.9%;
    margin-bottom: 1.4%;
}

.Active_address,
.Free_sent {
    float: left;
    width: 94.2%;
    padding: 3.3% 2.9%;
    margin-bottom: 1.4%;
}

.Content_info {
    float: left;
    width: 94.2%;
    padding: 2.1% 2.9% 4.1% 2.9%;
    border-bottom: 1px solid #f2f2f2;
}

.img_Title img {
    float: left;
    width: 100%;
}

.Tib_bar_p {
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

.Tib_bar_p img {
    width: 3.3%;
    margin-right: 1.1%
}

.Tib_bar_p div {
    margin-top: -1px;
    margin-right: 3.2%
}
















/*标题栏*/

.Like_cover_3 {
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
    align-items: flex-end;
    -webkit-align-items: flex-end;
    font-weight: bold;
    line-height: 15px;
    padding: 3% 0;
    font-size: 14px;
    border-bottom: 1px solid #f5f5f5
}

.level_Area div {
    display: flex;
    display: -webkit-flex;
    color: #333333;
    font-weight: normal;
    margin-right: 5px
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

.count_down_box {
    display: flex;
    display: -webkit-flex;
    flex: 1;
    -webkit-flex: 1;
    justify-content: flex-end;
    -webkit-justify-content: flex-end;
    line-height: 38px;
    font-size: 12px;
    color: #666666;
    align-items: center;
    -webkit-align-items: center;
}

.count_down_box div {
    width: 30px;
    height: 30px;
    line-height: 30px;
    text-align: center;
    background-color: #000000;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    margin: 0 5px;
    font-size: 16px;
    color: #ffffff
}















/*免费服务*/

.Free_sent {
    align-items: center;
    -webkit-align-items: center;
    justify-content: space-between;
    -webkit-justify-content: space-between;
}
















/*举办地*/

.Active_address {
    font-size: 14px;
    font-weight: bold;
    color: #333333
}

.Active_address div {
    flex: 1;
    -webkit-flex: 1;
}

.Active_address img {
    float: right;
    height: 14px;
    margin: 3px 0 0 5px;
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

.Content_info_box,
.painter_s {
    float: left;
    width: 100%;
    font-size: 16px;
    /*text-indent: 2em;*/
    /* line-height: 1.5em;
    letter-spacing: 2px;
    word-spacing: 4px; */
    color: #666666;
    overflow-x: hidden
}

.painter_s div,
.painter_s p {
    float: left;
    width: 100%;
    max-width: 100%;
    height: auto;
}

.painter_s img {
    float: left;
    width: 100%;
    max-width: 100%;
    height: auto;
    padding: 3.3% 0 2.7% 0;
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
    font-weight: bold;
    padding: 11px 0;
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
    height: 49px;
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
    font-size: 18px;
    color: #ff3f5c;
    font-weight: bold;
    line-height: 18px;
}

.Charge_info div:last-child {
    text-align: center;
    font-size: 12px;
    color: #999999;
    line-height: 14px;
}
.May_List{
    width: 100%;
    margin-top: -6.5%;
    margin-bottom: 60px;
}
</style>
