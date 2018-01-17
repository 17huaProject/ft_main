<template>
    <div class="boxArea">
        <div class="mencCles" @click="GoDetail" v-show="message.event_status=='SOLDOUT'">
             <div class="mengcCbox">
                <div>已售完</div>
            </div>
        </div>
        <div class="img_top_area">
            <div class="this_acTime_floatTop">
                <div>{{getTime(message.event_time,0)}}</div>
                <div>{{getTime(message.event_time,1)}}</div>
            </div>
            <img @click="GoDetail" :src="message.event_img.indexOf('http')<0?this.$store.state.SyetemBaseConfig.image_prefix+message.event_img:''+message.event_img.replace(/\http:/g,'')">
        </div>
        <div class="LineInfo">
            <img v-show="limit!=''" src="../../../static/obj_img/list_img/list_location_icon.png">
            <div v-show="limit!=''" >距离{{limit}}</div>
            <img v-show="message.event_status!='SOLDOUT'" src="../../../static/obj_img/list_img/list_ticket_icon.png">
            <div v-show="message.event_status!='SOLDOUT'">余票{{message.left_num}}张</div>
            <!-- <img src="../../../static/obj_img/list_img/list_time_icon.png">
                <div></div> -->
        </div>
        <div class="Active_info_area" @click="GoDetail">
            <div class="title_active">{{message.event_name}}</div>
            <div class="address_active">
                <div>{{message.venue_name}}</div>
                <img src="../../../static/obj_img/list_img/list_seratch_icon_2.png">
            </div>
        </div>
        <div class="Active_penterArea" @click="GoDetail">
            <img class="penterArea_img" :src="message.artist_avatar.indexOf('http')<0?this.$store.state.SyetemBaseConfig.image_prefix+message.artist_avatar:''+message.artist_avatar.replace(/\http:/g,'')">
            <div class="penterArea_info">
                <div>{{message.artist_name}}</div>
                <div>
                    <img class="star_af" :src="tipsLevel(message.artist_level, 0)">
                    <img class="star_af" :src="tipsLevel(message.artist_level, 1)">
                    <img class="star_af" :src="tipsLevel(message.artist_level, 2)">
                    <img class="star_af" :src="tipsLevel(message.artist_level, 3)">
                    <img class="star_af" :src="tipsLevel(message.artist_level, 4)">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            Like: true,
            address_mess: '',
            limit: this.setFarlimit(this.message.score)
        }
    },
    methods: {
        LikeThisActivity(like) {
            this.Like = like
        },
        GoDetail() {
            TDAPP.onEvent('活动列表点击', '列表进入', '活动id' + this.message.id)
            this.$router.push({ name: 'acinfo', query: { actinfo: this.message.id } })
        },
        GoADetail() {
            this.$router.push({ name: 'ptinfo', query: { prtinfo: this.message.artist_id } })
            // this.$router.push({ path: '/pt' })
        },
        GoADDetail() {
            this.$router.push({ name: 'atinfo', query: { addinfo: this.message.venue_id } })
        },
        setFarlimit(data) {
            if (data > 50) {
                return '大于50km'
            } else if (data > 1 && data <= 50) {
                return parseInt(data) + 'km'
            } else if (data < 1 && data * 1000 > 23) {
                return parseInt(data * 1000) + 'm'
            } else {
                return ''
            }
        },
        getTime(time, index) {
            time = time.replace(/\-/g, '/')
            let t = new Date(time)
            const weekName = ['日', '一', '二', '三', '四', '五', '六'];
            return index == 0 ? (1 + t.getMonth()) + '月' + t.getDate() + '日' : '周' + weekName[t.getDay()] + ' ' + (t.getHours() < 10 ? '0' + t.getHours() : t.getHours()) + ':' + (t.getMinutes() < 10 ? '0' + t.getMinutes() : t.getMinutes())
        },
        tipsLevel(num, ad) {
            if (num - ad >= 1) {
                return '../../../static/obj_img/list_img/list_Star_icon_1.png'
            } else if (num - ad > 0 && num - ad < 1) {
                return '../../../static/obj_img/list_img/list_Star_icon_2.png'
            } else {
                return '../../../static/obj_img/list_img/list_Star_icon_3.png'
            }
        },
    },
    props: ["message"],
    computed: {

    }
}
</script>

<style>
.boxArea {
    position: relative;
    float: left;
    width: 80%;
    margin: 10% 10% 0 10%;
    display: flex;
    display: -webkit-flex;
    -webkit-border-radius: 10px;
    border-radius: 10px;
    overflow: hidden;
    flex-direction: column;
    -webkit-flex-direction: column;
    background-color: #ffffff;
}







/*背景图部分*/

.img_top_area {
    float: left;
    width: 100%;
}

.img_top_area img {
    float: left;
    width: 100%;
}

.Like_cover {
    float: right;
    margin-left: -15.8%;
    width: 7.7%;
    padding: 4.1% 3.5%;
    margin-bottom: -2%;
}










/*信息栏*/

.LineInfo {
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

.LineInfo img {
    width: 3.3%;
    margin-right: 1.1%
}

.LineInfo div {
    margin-top: -1px;
    margin-right: 3.2%
}




/*活动内容部分*/

.Active_info_area {
    width: 92%;
    background-color: #ffffff;
    margin: 0 4%;
    padding-bottom: 10px;
    border-bottom: 1px solid #eeeeee
}

.title_active {
    font-size: 16px;
    font-weight: bold;
    padding-top: 10px;
    color: #333333;
}

.address_active {
    width: 100%;
    height: 12px;
    float: left;
    color: #666666;
    font-size: 12px;
    padding-top: 10px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

.address_active div {
    flex: 1;
    -webkit-flex: 1;
    height: 20px;
    line-height: 19px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.address_active img {
    margin-top: 2px;
    float: right;
    height: 12px;
}
















/*画师部分*/

.Active_penterArea {
    width: 92%;
    background-color: #ffffff;
    padding: 0 4%;
    padding: 10px 4%;
    display: flex;
    display: -webkit-flex;
}

.penterArea_img {
    -webkit-border-radius: 20px;
    border-radius: 20px;
    width: 30px;
    height: 30px;
    margin-right: 3.3%;
}

.penterArea_info {
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

.penterArea_info div {
    font-size: 14px;
    height: 14px;
    line-height: 14px;
    color: #666666;
    margin-right: 5px;
}

.penterArea_info div img {
    height: 14px;
    margin: 0 0 0 2px;
}

.star_af {
    width: 14px;
    margin: 8px 4px 0 0;
}

.this_acTime_floatTop {
    float: left;
    padding: 0 8px;
    background-color: #ff3f5c;
    color: #ffffff;
    text-align: center;
    font-size: 12px;
    line-height: 23px;
    height: 46px;
    position: relative;
    margin: 0 0 -46px 5%;
}

.mencCles {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 2;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
}

.mengcCbox{
    width: 80px;
    height: 80px;
    border-radius: 80px;
    box-shadow: 0 0 0 2px #ffffff;
    overflow: hidden;
    transform: rotateZ(-30deg)
}
.mengcCbox div{
    width: 90px;
    height: 20px;
    margin: 30px -5px;
    text-align: center;
    line-height: 20px;
    background-color: #ffffff;
    color: #999999;
    font-size: 12px;
    font-weight: bold;
}
</style>
