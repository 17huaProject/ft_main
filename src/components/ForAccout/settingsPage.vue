<template>
    <div id="SettingsArea">
        <div class="cle_line" @click="routerGo('/invOLs')">
            <img src="../../../static/obj_img/accout_img/invoice_invoice_icon.png">
            <div>发票与报销</div>
            <img src="../../../static/obj_img/accout_img/my_purse_icon.png">
        </div>
        <div class="cle_line" @click="routerGo('/cl')">
            <img src="../../../static/obj_img/accout_img/my_linkman_icon.png">
            <div>常用信息</div>
            <img src="../../../static/obj_img/accout_img/my_purse_icon.png">
        </div>
        <div class="cle_line" @click="routerGo('/faq')">
            <img src="../../../static/obj_img/more_img/more_problem_icon.png">
            <div>常见问题</div>
            <img src="../../../static/obj_img/accout_img/my_purse_icon.png">
        </div>
        <div class="cle_line" @click="routerGo('/fb')">
            <img src="../../../static/obj_img/more_img/more_jianyi_icon.png">
            <div>意见反馈</div>
            <img src="../../../static/obj_img/accout_img/my_purse_icon.png">
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        routerGo: function (paths) {
            this.$router.push({ path: paths })
        },
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

#SettingsArea {
    width: 100%;
    float: left;
}

.cle_line {
    float: left;
    width: 93.4%;
    padding: 4% 3.3%;
    display: flex;
    display: -webkit-flex;
    background-color: #ffffff;
    margin-bottom: 1%;
}

.cle_line img {
    float: left;
    margin-top: 3px;
    height: 16px;
}

.cle_line img:first-child {
    margin-right: 3.7%;
    height: 20px;
    margin-top: 1px;
}

.cle_line div {
    flex: 1;
    -webkit-flex: 1;
    color: #333333;
}
</style>
