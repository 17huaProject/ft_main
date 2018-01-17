<template>
    <div id="morepage">
        <div class="serverBox">
            <!-- <div class="serverOnline">
                                <img src="../../static/obj_img/more_img/more_serivice_icn1.png"> 在线客服
                            </div> -->
            <a href="tel:02167991260">
                <div class="serverPhone">
                    <img src="../../static/obj_img/more_img/more_serivice_icn2.png"> 客服热线
                </div>
            </a>
    
        </div>
        <div class="OtherLine" @click="routerGo('/au')">
            <img src="../../static/obj_img/more_img/more_About_icon.png">
            <div>关于我们</div>
            <img src="../../static/obj_img/list_icon_arrRight.png">
        </div>
        <div class="OtherLine" @click="routerGo('/faq')">
            <img src="../../static/obj_img/more_img/more_problem_icon.png">
            <div>常见问题</div>
            <img src="../../static/obj_img/list_icon_arrRight.png">
        </div>
        <div class="OtherLine" @click="routerGo('/fb')">
            <img src="../../static/obj_img/more_img/more_jianyi_icon.png">
            <div>意见反馈</div>
            <img src="../../static/obj_img/list_icon_arrRight.png">
        </div>
        <div id="listBottom_more">
            <div @click="routerGo('/list')">
                <img src="../../static/obj_img/tab_list_normal_icon.png">
                <div>活动</div>
            </div>
            <div @click="routerGo('/ac')">
                <img src="../../static/obj_img/tab_more_nomal_icon.png">
                <div>我的</div>
            </div>
            <div>
                <img src="../../static/obj_img/tab_more_selected_icon.png">
                <div style="color:#ff3f5c">更多</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        routerGo: function (paths) {
            this.$router.push({ path: paths })
        }
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

a {
    width: 100%;
    text-decoration: none
}

#morepage {
    width: 100%;
    float: left;
    height: 100vh
}

.serverBox {
    float: left;
    width: 100%;
    padding: 4.3% 0;
    display: flex;
    display: -webkit-flex;
    background-color: #ffffff;
}

.serverOnline,
.serverPhone {
    flex: 1;
    -webkit-flex: 1;
    padding: 3.3%;
    color: #333333;
    font-size: 16px;
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
}

.serverOnline {
    border-right: 1px solid #f5f5f5;
}

.serverPhone {
    border-left: 1px solid #f5f5f5;
}

.serverOnline img,
.serverPhone img {
    height: 24px;
    margin-right: 1%;
}

.OtherLine {
    float: left;
    width: 93.4%;
    padding: 4.3% 3.3%;
    margin-top: 1%;
    background-color: #ffffff;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

.OtherLine img:first-child {
    height: 24px;
    margin-right: 3.3%;
}

.OtherLine img:last-child {
    margin-top: 2px;
    height: 14px;
}

.OtherLine div {
    flex: 1;
    -webkit-flex: 1;
}
#listBottom_more {
    position: absolute;
    left: 0;
    bottom: 0;
    background-color: #ffffff;
    width: 100%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: space-between;
    -webkit-justify-content: space-between;
    box-shadow: 0 0 5px 1px #f1f1f1
}

#listBottom_more div {
    padding: 5px 5% 0 5%;
    width: 50px;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
     align-items: center;
    -webkit-align-items: center;
}

#listBottom_more div div {
    -webkit-transform: scale(0.8);
    font-size: 12px;
}

#listBottom_more img {
    width: 24px;
    height: 24px;
    margin-bottom: -5px;
}
</style>
