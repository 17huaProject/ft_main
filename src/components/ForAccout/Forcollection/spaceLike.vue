<template>
    <div class="mainActiveLike">
        <div class="space_box" :key="'sp'+item.venue_id" v-for="(item, index) in list_faverate_d" @click="pathTo(item.venue_id)">
            <img class="spanImg_de" :src="item.big_img.indexOf('http')<0?$store.state.SyetemBaseConfig.image_prefix+item.big_img:''+item.big_img.replace(/\http:/g,'')">
            <div class="space_name">{{item.venue_name}}</div>
            <div class="space_info">
                简介：{{item.venue_desc}}
            </div>
        </div>
        <div v-show="list_faverate_d.length==0" style="width:100%;line-height:50px;text-align:center;color:#999999">· 没有收藏地点哦 ·</div>
    </div>
</template>

<script>
export default {
    // data(){
    //     return{
    //         spaceArr:[
    //             {name:'名字12',img:'../../../../static/obj_img/accout_img/my_collect_place.png',infosa:'洋溢着艺术气息的都市花园'},
    //             {name:'名字13',img:'../../../../static/obj_img/accout_img/my_collect_place.png',infosa:'洋溢着艺术气息的都市花'},
    //             {name:'名字14',img:'../../../../static/obj_img/accout_img/my_collect_place.png',infosa:'洋溢着艺术气息的都市'},
    //             {name:'名字15',img:'../../../../static/obj_img/accout_img/my_collect_place.png',infosa:'洋溢着艺术气息的都'},
    //             {name:'名字16',img:'../../../../static/obj_img/accout_img/my_collect_place.png',infosa:'洋溢着艺术气息的'},
    //             ]
    //     }
    // }
    data() {
        return {
            list_faverate_d: []
        }
    },
    created() {
        let f = {
            type: 'VENUE',
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
                this.list_faverate_d = data.records
            } else {
                this.$toast.center(data.errmsg)
            }
        },
        pathTo(mus) {
            this.$router.push({ name: 'atinfo', query: { addinfo: mus } })
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

.space_box {
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

.space_name {
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

.space_info {
    width: 92%;
    padding: 0 4% 4% 4%;
    float: left;
    color: #666666;
    font-size: 12px;
    text-align: justify;
    text-justify: distribute-all-lines;
}


/* .space_info div:last-child {
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
</style>
