<template>
    <div class="detail_info">
        <!-- <img class="Like_cover_3" v-show="Like" @click="LikeThisActivity(false)" src="../../static/obj_img/list_Collect_icon_.png">
        <img class="Like_cover_3" v-show="!Like" @click="LikeThisActivity(true)" src="../../static/obj_img/list_Collected_icon.png"> -->
        <div class="img_Title">
            <img src="../../static/obj_img/temple_img/activity_step_photo_02.png">
        </div>
        <div class="MainTitle_bar">
            <div class="blog_altic">
                <img class="blog_altic_img" src="../../static/obj_img/temple_img/list_.writer_photo.png">
                <div class="blog_altic_name">
                    {{blogInfo.author}}
                </div>
                <img class="blog_altic_eye" src="../../static/obj_img/list_img/list_read_icon_2.png">
                <div class="blog_altic_seenumber">{{blogInfo.hits}}</div>
            </div>
            <div class="address_title">{{blogInfo.title}}</div>
            <div class="altic_painter painter_s" v-html="blogInfo.introduce">
               
            </div>
             <div class="painter_s" v-html="blogInfo.content">
                
            </div>
        </div>
        <div class="Introduce_Like">
            <div class="May_Like">你应该会喜欢</div>
            <div class="May_List">
                <areabox></areabox>
            </div>
        </div>
    </div>
</template>

<script>
import ActiveBox from './ForList/ListBarStyle_1.vue'
export default {
    data() {
        return {
            Like: true,
            money: 99,
            JoinNum: 20,
            CD_day: 2,
            CD_hour: '06',
            CD_mini: 14,
            ac_ad: '上海市浦东新区芳甸路333号喜马拉雅中心B1',
            blogInfo:{}
        }
    },
    created() {
        this.getBDetailMaster()
    },
    methods: {
        // LikeThisActivity: function (like) {
        //     this.Like = like
        // },
        getBDetailMaster() {
            let idmas = {
                id:parseInt(this.$route.query.bloInfo) || 0,
            }
            this.$store.commit('ConectionPara', idmas)
            let getString = this.$store.state.getParamString;
            let postString = getString.replace('?', '');
            this.$http.get(this.$store.state.serverHost + '/yqhbsp/article/show' + getString).then(m => this.setBlog(m.data)).catch(r => console.log(r));
        },
        unescape(html) {
            html = this.$store.state.PROTOCOL?html.replace(/\http:/g,'https:'):html
            return html
                .replace(html ? /&(?!#?\w+;)/g : /&/g, '&amp;')
                .replace(/&lt;/g, "<")
                .replace(/&gt;/g, ">")
                .replace(/&quot;/g, "\"")
                .replace(/&#39;/g, "\'")
                .replace(/&mdash;/g, "-")
                .replace(/&nbsp;/g, " ")
        },
        setBlog(data) {
            if (data.errcode === 200) {
                data.record.is_faverate==1?this.likeId=data.record.faverate.id:'';
                this.blogInfo = data.record;
            }else {
                this.$toast.center(data.errmsg)
            }
        },

    },
    components: {
        areabox: ActiveBox,
    }
}
</script>

<style>
body {
    background-color: #f5f5f5
}
.blog_altic_img{
    width: 12%;
    -webkit-border-radius:100px;
    border-radius:100px;
}
.blog_altic_name{
    margin-left: 3.3%;
    flex: 1;
    -webkit-flex:1;
}
.blog_altic_eye{
     width: 5%;
     margin-right: 3.3%;
}
.altic_painter {
    padding: 3.7% 0;
    margin-top: 3.7%;
}
.altic_painter img{
    width: 100%;
    margin: 2.1% 0;
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

.blog_altic {
    float: left;
    display: flex;
    display: -webkit-flex;
    width: 100%;
    padding: 3.3% 0;
    align-items: center;
    -webkit-align-items: center;
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
    flex: 1;
    -webkit-flex:1
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
    border-top: 1px solid #f2f2f2;
    float: left;
    width: 100%;
    font-weight: bold;
    padding-top: 2.7%;
    font-size: 18px;
    color: #333333;
}

.address_info {
    float: left;
    width: 100%;
    color: #333333;
    padding-top: 3%;
    font-size: 14px;
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
    color: #ff7d54;
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
    font-size: 16px;
    text-indent: 2em;
    line-height: 1.5em;
    letter-spacing: 2px;
    word-spacing: 4px;
    color: #666666;
}

.Content_info_box img {
    width: 100%;
    margin: 10px 0;
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
    color: #ff7d54;
    font-weight: bold;
    line-height: 18px;
}

.Charge_info div:last-child {
    text-align: center;
    font-size: 12px;
    color: #999999;
    line-height: 14px;
}
</style>
