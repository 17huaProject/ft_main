<template>
    <div class="mainActiveLike">
        <div class="painter_box" :key="'pt'+item.artist_id" v-for="(item, index) in list_faverate_p" @click="pathTo(item.artist_id)">
            <img class="spanImg_de" v-bind:src="item.artist_avatar.indexOf('http')<0?$store.state.SyetemBaseConfig.image_prefix+item.artist_avatar:''+item.artist_avatar.replace(/\http:/g,'')">
            <div class="painter_name">{{item.artist_name}}</div>
            <div class="painter_info">
                简介：{{item.artist_desc}}
            </div>
            <div class="painter_level">
                <div>评级</div>
                <img :src="DrawLevel(item.artist_level,0)">
                <img :src="DrawLevel(item.artist_level,1)">
                <img :src="DrawLevel(item.artist_level,2)">
                <img :src="DrawLevel(item.artist_level,3)">
                <img :src="DrawLevel(item.artist_level,4)">
            </div>
        </div>
        <div v-show="list_faverate_p.length==0" style="width:100%;line-height:50px;text-align:center;color:#999999">· 没有收藏画师哦 ·</div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            list_faverate_p: []
        }
    },
    created() {
        let f = {
            type: 'ARTIST',
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
                this.list_faverate_p = data.records
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        pathTo(mus) {
            this.$router.push({ name: 'ptinfo', query: { prtinfo: mus } })
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

.painter_box {
    float: left;
    width: 96%;
    margin-top: 4%;
    padding-bottom: 10px;
    /* margin: 2% 2% 0 2%; */
    background-color: #ffffff;
    -webkit-border-radius: 2px;
    border-radius: 2px;
    -webkit-column-break-inside: avoid;
    break-inside: avoid;
}

.painter_name {
    font-weight: bold;
    width: 92%;
    padding: 4%;
    float: left;
    font-size: 14px;
}

.spanImg_de {
    width: 100%;
    float: left;
}

.painter_info {
    width: 92%;
    padding: 0 4% 4% 4%;
    color: #666666;
    float: left;
    font-size: 12px;
    text-align:justify; 
    text-justify:distribute-all-lines; 
}


/* .painter_info div:last-child {
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    height: 40px;
    line-height: 20px;
} */

.painter_level {
    width: 92%;
    padding: 0 4% 4% 4%;
    float: left;
    font-size: 12px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

.painter_level img {
    height: 12px;
    margin-left: 2px;
}
</style>
