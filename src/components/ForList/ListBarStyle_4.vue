<template>
    <div class="boxArea_4">
        <div class="mencCles" @click="GoDetail" v-if="message.count==0">
            <div class="mengcCbox">
                <div>无货</div>
            </div>
        </div>
        <img class="img_top_area_4" @click="GoDetail" :src="message.goods_img.replace(/\http:/g,'')">
        <div class="Active_info_area_4">
            <div class="title_active_4">{{message.name}}</div>
        </div>
        <div class="Active_penterArea_4">
            <div class="penterArea_info_4">
                <div class="Pd_sale">￥{{message.price}}</div>
                <div class="Pd_sale_s">&nbsp;原价￥{{message.sale_price}}&nbsp;</div>
            </div>
            <!-- <div class="penterArea_info_4" v-if="message.count==1000">
                无货
            </div> -->
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {

        }
    },
    methods: {
        GoDetail: function() {
            this.$router.push({ name: 'pdinfo', query: { pdcinfo: this.message.id } })
        },
    },
    props: ["message"],
}
</script>

<style>
.boxArea_4 {
    -webkit-column-break-inside: avoid;
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
    position: relative;
}

.mencCles {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
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
/*背景图部分*/

.img_top_area_4 {
    float: left;
    width: 100%;
}

.Like_cover_4 {
    float: right;
    width: 7.7%;
    padding: 4.1% 3.5%;
}










/*信息栏*/

.LineInfo_4 {
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

.LineInfo_4 img {
    width: 8%;
    margin-right: 2.4%
}

.LineInfo_4 div {
    margin-top: -1px;
}










/*活动内容部分*/

.Active_info_area_4 {
    width: 92%;
    background-color: #ffffff;
    margin: 0 4%;
    padding-bottom: 10px;
}

.title_active_4 {
    font-size: 14px;
    font-weight: bold;
    padding-top: 10px;
    color: #333333;
}

.address_active_4 {
    width: 100%;
    float: left;
    color: #666666;
    font-size: 14px;
    padding-top: 10px;
}

.address_active_4 img {
    margin-top: 2px;
    float: right;
    height: 16px;
}









/*画师部分*/

.Active_penterArea_4 {
    border-top: 1px solid #f2f2f2;
    width: 92%;
    background-color: #ffffff;
    margin: 0 4%;
    padding: 10px 0;
    display: flex;
    display: -webkit-flex;
}

.penterArea_img_4 {
    -webkit-border-radius: 10px;
    border-radius: 10px;
    width: 20px;
    height: 20px;
    margin-right: 3.3%;
}

.penterArea_info_4 {
    width: 100%;
    line-height: 20px;
    font-size: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between
}

.Pd_sale{
    color: #ff3f5c;
    font-size: 18px;
    margin: 0 10px 0 0;
    display: flex;
    align-items: center;
}

.Pd_sale_s{
    color: #666666;
    font-size: 12px;
    text-decoration: line-through;
}
</style>
