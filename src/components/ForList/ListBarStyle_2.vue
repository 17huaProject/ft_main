<template>
    <div class="boxArea_2">
        <img class="img_top_area_2" @click="GoDetail" src="../../../static/obj_img/temple_img/activity_guest_photo_05.png">
        <div class="LineInfo_2">
            <img src="../../../static/obj_img/list_img/list_ticket_icon.png">
            <div>已售完</div>
            <div class="poAler"></div>
            <img src="../../../static/obj_img/list_img/list_read_icon_1.png">
            <div>660</div>
        </div>
        <div class="Active_info_area_2" @click="GoADetail">
            <div class="title_active_2">活动标题</div>
            <div class="address_active_2">地址:{{address_mess}}</div>
        </div>
        <div class="Active_penterArea_2" @click="GoADDetail">
            <img class="penterArea_img_2" src="../../../static/obj_img/temple_img/activity_paiter_photo.png">
            <div class="penterArea_info_2">
                <div>画师姓名</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            address_mess: '上海市喜马拉雅芳甸路1188号B1'
        }
    },
    methods: {
        GoDetail: function () {
            this.$router.push({ path: '/dt' })
        },
        GoADetail: function () {
            this.$router.push({ path: '/pt' })
        },
        GoADDetail: function () {
            this.$router.push({ path: '/at' })
        }
    }
}
</script>

<style>
.boxArea_2 {
    -webkit-column-break-inside: avoid;
    width: 100%;
    margin: 0 0 7.5% 0;
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

.img_top_area_2 {
    float: left;
    width: 100%;
}

.Like_cover_2 {
    float: right;
    width: 7.7%;
    padding: 4.1% 3.5%;
}










/*信息栏*/

.LineInfo_2 {
    float: left;
    width: 92%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    font-size: 12px;
    color: #ffffff;
    height: 30px;
    margin-top: -30px;
    padding: 0 4%
}

.poAler {
    flex: 1;
    -webkit-flex: 1;
}

.LineInfo_2 img {
    width: 8%;
    margin-right: 2.4%
}

.LineInfo_2 div {
    margin-top: -1px;
}










/*活动内容部分*/

.Active_info_area_2 {
    width: 92%;
    background-color: #ffffff;
    margin: 0 4%;
    padding-bottom: 10px;
}

.title_active_2 {
    font-size: 14px;
    font-weight: bold;
    padding-top: 10px;
    color: #333333;
}

.address_active_2 {
    width: 100%;
    float: left;
    color: #666666;
    font-size: 14px;
    padding-top: 10px;
}

.address_active_2 img {
    margin-top: 2px;
    float: right;
    height: 16px;
}









/*画师部分*/

.Active_penterArea_2 {
    border-top: 1px solid #f2f2f2;
    width: 92%;
    background-color: #ffffff;
    margin: 0 4%;
    padding: 10px 0;
    display: flex;
    display: -webkit-flex;
}

.penterArea_img_2 {
    -webkit-border-radius: 10px;
    border-radius: 10px;
    width: 20px;
    height: 20px;
    margin-right: 3.3%;
}

.penterArea_info_2 {
    flex: 1;
    -webkit-flex: 1;
    flex-direction: column;
    -webkit-flex-direction: column;
    line-height: 20px;
    font-size: 12px;
}
</style>
