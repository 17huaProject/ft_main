<template>
    <div class="mainActiveLike">
        <div class="activelike_box" :key="'at'+item.event_id" v-for="(item,index) in list_faverate_a" @click="pathTo(item.event_id)">
            <img class="activeLike_img" :src="item.big_img.indexOf('http')<0?$store.state.SyetemBaseConfig.image_prefix+item.big_img:''+item.big_img.replace(/\http:/g,'')">
            <div class="activeLike_name">{{item.event_name}}</div>
            <div class="activeLike_info">{{item.event_desc}}</div>
            <!-- <div class="activeLike_info">
                    <div class="al_info_right">
                        <div>活动时间：{{item.event_time}}</div>
                        <div>地址：{{item.address}}</div>
                        
                    </div>
                </div> -->
            <div class="activeLike_level">难度
                <img :src="DrawLevel(item.easy_level,0)">
                <img :src="DrawLevel(item.easy_level,1)">
                <img :src="DrawLevel(item.easy_level,2)">
                <img :src="DrawLevel(item.easy_level,3)">
                <img :src="DrawLevel(item.easy_level,4)">
            </div>
        </div>
        <div v-show="list_faverate_a.length==0" style="width:100%;line-height:50px;text-align:center;color:#999999">· 没有收藏活动哦 ·</div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            list_faverate_a: []
        }
    },
    created() {
        let f = {
            type: 'EVENT',
            access_token: this.$store.state.usrInfomation.access_token
        }
        this.$store.commit('ConectionPara', f)
        let getString = this.$store.state.getParamString;
        let postString = getString.replace('?', '');
        this.$http.get(this.$store.state.serverHost + '/yqhbsp/faverate/index' + getString).then(m => this.getFavList(m.data)).catch(r => console.log(r));
    },
    methods: {
        DrawLevel(num, ad) {
            if (num - ad >= 1) {
                return '../../../../static/obj_img/list_img/list_Star_icon_1.png'
            } else if (num - ad > 0 && num - ad < 1) {
                return '../../../../static/obj_img/list_img/list_Star_icon_2.png'
            } else {
                return '../../../../static/obj_img/list_img/list_Star_icon_3.png'
            }
        },
        getFavList(data) {
            if (data.errcode === 200) {
                this.list_faverate_a = data.records
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        pathTo(mus) {
            this.$router.push({ name: 'acinfo', query: { actinfo: mus } })
        }
    }
}
</script>

<style>
.mainActiveLike {
    width: 98%;
    padding-left: 2%;
    float: left;
    -webkit-column-count: 2;
    column-count: 2;
    -webkit-column-gap: 0;
    column-gap: 0;
}

.activelike_box {
    /* margin-top: 1%;
    width: 94.2%;
    padding: 3.1% 3.1% 6.1% 3.1%; */
    float: left;
    width: 96%;
    margin-top: 4%;
    /* margin: 2% 2% 0 2%; */
    padding-bottom: 10px;
    background-color: #ffffff;
    -webkit-border-radius: 2px;
    border-radius: 2px;
    -webkit-column-break-inside: avoid;
    break-inside: avoid;
}

.activeLike_img {
    width: 100%;
    float: left;
}

.activeLike_name {
    font-weight: bold;
    width: 92%;
    padding: 4%;
    float: left;
    font-size: 14px;
}

.activeLike_info {
    width: 92%;
    padding: 4%;
    float: left;
    font-size: 12px;
    color: #666666;
}
.activeLike_level{
    width: 92%;
    padding:0 4% 4% 4%;
    float: left;
    font-size: 12px;
     display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}
.activeLike_level img{
    height: 12px;
    margin-left: 2px;
}
.activeLike_info img {
    width: 27%;
}
.al_info_right {
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    margin-left: 3.1%;
}

.al_info_right div:nth-child(2) {
    flex: 1;
    -webkit-flex: 1;
    padding: 4.1% 0;
}

.al_info_right div:last-child {
    display: flex;
    display: -webkit-flex;
    align-items: flex-end;
    -webkit-align-items: flex-end;
}

.al_info_right img {
    width: 14px;
    height: 14px;
    margin-left: 5px;
}
</style>
