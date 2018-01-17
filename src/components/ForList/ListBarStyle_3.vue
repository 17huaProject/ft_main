<template>
    <div class="boxArea_3">
        <img class="img_top_area_3" @click="GoDetail" src="../../../static/obj_img/temple_img/activity_xiangqing_photo.png">
        <div class="Active_info_area_3">
            <div class="title_active_3">{{message.title}}</div>
        </div>
        <div class="Active_penterArea_3" >
            <img class="penterArea_img_3" src="../../../static/obj_img/temple_img/list_.writer_photo.png">
            <div class="penterArea_info_3">
                <div>{{message.author}}</div>
            </div>
            <div class="poAler"></div>
            <img class="penterArea_img_3" src="../../../static/obj_img/list_img/list_read_icon_2.png">
            <div class="penterArea_info_3">
                <div>{{message.hits}}</div>
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
            this.$router.push({ name: 'blogD', query: { bloInfo: this.message.article_id } })
        },
        // GoADetail:function(){
        //     this.$router.push({ path: '/pt' })
        // }
    },
    props: ["message"],
}
</script>

<style>
.boxArea_3 {
    -webkit-column-break-inside:avoid;
    width: 100%;
    margin:  0 0 7.5% 0;
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

.img_top_area_3 {
    float: left;
    width: 100%;
}

.Like_cover_3 {
    float: right;
    width: 7.7%;
    padding: 4.1% 3.5%;
}









/*信息栏*/

.LineInfo_3 {
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
.poAler{
    flex: 1;
    -webkit-flex: 1;
}
.LineInfo_3 img {
    width: 8%;
    margin-right: 2.4%
}

.LineInfo_3 div {
    margin-top: -1px;
}









/*活动内容部分*/

.Active_info_area_3 {
    width: 92%;
    background-color: #ffffff;
    margin: 0 4%;
    padding-bottom: 10px;
    
}

.title_active_3 {
    font-size: 14px;
    font-weight: bold;
    padding-top: 10px;
    color: #333333;
}

.address_active_3 {
    width: 100%;
    float: left;
    color: #666666;
    font-size: 14px;
    padding-top: 10px;
}

.address_active_3 img {
    margin-top: 2px;
    float: right;
    height: 16px;
}








/*画师部分*/

.Active_penterArea_3 {
    border-top: 1px solid #f2f2f2;
    width: 92%;
    background-color: #ffffff;
    margin: 0 4%;
    padding: 10px 0;
    display: flex;
    display: -webkit-flex;
}

.penterArea_img_3 {
    -webkit-border-radius: 10px;
    border-radius: 10px;
    width: 20px;
    height: 20px;
    margin-right: 3.3%;
}

.penterArea_info_3 {
    line-height: 20px;
    font-size: 12px;
}
</style>
