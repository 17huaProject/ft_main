<template>
    <div class="detail_info">
        <img class="Like_cover_3" v-show="likeId==''" @click="conectionBYid(true)" src="../../static/obj_img/list_Collect_icon_.png">
        <img class="Like_cover_3" v-show="likeId!=''" @click="conectionBYid(false)" src="../../static/obj_img/list_Collected_icon.png">
        <div class="img_Title">
            <img :src="addrInfo.big_img.indexOf('http')<0?this.$store.state.SyetemBaseConfig.image_prefix +addrInfo.big_img:''+addrInfo.big_img.replace(/\http:/g,'')">
        </div>
        <div class="MainTitle_bar">
            <div class="address_title">{{addrInfo.venue_name}}</div>
            <div class="address_info" @click="jumpToamap">{{addrInfo.address}}<img src="../../static/obj_img/list_icon_arrRight.png"></div>
        </div>
        <div class="MainTitle_bar">
            <!--<div class="altic_painter addresser_s" v-html="addrInfo.venue_desc">
    
            </div>-->
            <div class="addresser_s" v-html="this.unescape(addrInfo.content)">
    
            </div>
        </div>
        <div class="Introduce_Like">
            <div class="May_Like">此地点即将举办的活动</div>
            <div class="May_List">
                <areabox :key="listArr[index].id" v-for="(item, index) in listArr" :message='listArr[index]'></areabox>
            </div>
        </div>
    </div>
</template>

<script>
import ActiveBox from './ForList/ListBarStyle_1.vue'
export default {
    data() {
        return {
            addressID: {
                id: parseInt(this.$route.query.addinfo) || 0,
                access_token: this.$store.state.usrInfomation.access_token || ''
            },
            addrInfo: {},
            likeId: '',
            money: 99,
            JoinNum: 20,
            CD_day: 2,
            CD_hour: '06',
            CD_mini: 14,
            ac_ad: '上海市浦东新区芳甸路333号喜马拉雅中心B1',
            listArr: []
        }
    },
    created() {
        this.getADetailMaster()
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
        getADetailMaster() {
            this.$store.commit('ConectionPara', this.addressID)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/venue/show' + getString).then(m => this.setAdder(m.data)).catch(r => console.log(r));
        },
        setAdder(data) {
            if (data.errcode === 200) {
                data.record.is_faverate == 1 ? this.likeId = data.record.faverate.id : '';
                this.addrInfo = data.record.venue;
                this.getListFot()
                // let share = {
                //     sharetitle: this.addrInfo.venue_name,
                //     sharelink: 'wx.17hua.me/at?addinfo=' + this.$route.query.addinfo,
                //     shareimgUrl: this.$store.state.SyetemBaseConfig.image_prefix+this.addrInfo.big_img,
                //     shareDesc: '来这儿和我一起画吧！'
                // }
                // this.$store.commit('useShareFun', share)
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        jumpToamap() {
            this.$router.push({ name: 'amapif', query: { lginfo: (this.addrInfo.latitude + ',' + this.addrInfo.longitude), lgname: this.addrInfo.address } })
        },
        conectionBYid(statue) {
            if (statue) {
                let thisCC = {
                    type: 'VENUE',
                    related_id: this.$route.query.addinfo,
                    access_token: this.$store.state.usrInfomation.access_token
                }
                this.$store.commit('ConectionPara', thisCC)
                let getString = this.$store.state.getParamString;
                let postString = getString.replace('?', '');
                this.$http.get(this.$store.state.serverHost + '/yqhbsp/faverate/add' + getString).then(m => this.LikeThisActivity(m.data)).catch(r => console.log(r));
            } else {
                let thisCC = {
                    type: 'VENUE',
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
                p: '',
                d: 500,
                stext: this.addrInfo.venue_name,
                start: this.YYYYMMdd(new Date()),
                end: this.YYYYMMdd(new Date(new Date().getTime() + parseInt(7 * 24 * 60 * 60 * 1000))),
                num: 1,
                page_no: 1,
                page_size: 5,
                sort: 'distance|0'
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
        //     sharetitle: this.addrInfo.venue_name,
        //     sharelink: 'wx.17hua.me/at?addinfo=' + this.$route.query.addinfo,
        //     shareimgUrl: this.$store.state.SyetemBaseConfig.image_prefix + this.addrInfo.big_img,
        //     shareDesc: '来这儿和我一起画吧！'
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
    margin-bottom: 5%;
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
    padding: 0 2.9%;
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

.Tib_bar {
    float: left;
    width: 92%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    align-items: center;
    -webkit-align-items: center;
    font-size: 12px;
    color: #ffffff;
    margin-top: -40px;
    padding: 0 4%
}

.Tib_bar div {
    flex: 1
}















/*标题栏*/

.Like_cover_3 {
    position: absolute;
    right: 0;
    top: 0;
    width: 7.2%;
    margin: 3.8% 3.2%;
}

.address_title {
    float: left;
    width: 100%;
    font-weight: bold;
    padding: 2.7% 0;
    font-size: 18px;
    color: #333333;
    border-bottom: 1px solid #eeeeee
}

.address_info {
    float: left;
    width: 100%;
    color: #333333;
    padding: 2.7% 0;
    font-size: 14px;
    font-weight: bold;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: space-between;
    -webkit-justify-content: space-between;
}
.address_info img{
    height: 12px;
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
.addresser_s {
    float: left;
    width: 100%;
    font-size: 16px;
    /*text-indent: 2em;*/
    /* line-height: 1.5em;
    letter-spacing: 2px;
    word-spacing: 4px; */
    padding: 3.3% 0;
    color: #666666;
    overflow-x: hidden
}

.addresser_s div,
.addresser_s p {
    float: left;
    width: 100%;
    max-width: 100%;
    height: auto;
}

.addresser_s img {
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
