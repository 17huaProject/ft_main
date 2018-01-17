<template>
    <div class="CityShow_area">
        <div class="CityShow" @click="toChangeCity()" v-show="modelNum">
            <div>{{this.$store.state.SelectCity}}</div>
            <img src="../../../static/obj_img/home_icon_ArrDown.png">
        </div>
        <div class="CityShow_2" @click="toChangeCity()" v-show="!modelNum">
            <div style="text-decoration: none">{{this.$store.state.SelectCity}}</div>
            <img src="../../../static/obj_img/list_img/list_seratch_icon_1.png">
        </div>
    </div>
</template>

<script>
// import CenterTrunk from '../../assets/eventBus'
export default {
    data() {
        return {
            modelNum: this.cityModel
        }
    },
    computed:{
        
    },
    methods: {
        toChangeCity() {
            this.$router.push({ path: '/cc' })
        }
    },
    mounted() {
        // var _self = this;
        // CenterTrunk.$on('isThisCity', function (name) {
        //     _self.Ccity = name;
        // });
    },
    props: ["cityModel"]
}
</script>

<style>
.CityShow_area {
    text-decoration: none;
    min-width: 60px;
}




/*首页*/

.CityShow {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    color: #fff;
    font-size: 16px;
    width: 25%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    padding: 3.8% 0;
}

.CityShow div {
    margin-left: 15.2%;
}

.CityShow img {
    width: 24%;
    margin-left: 7.6%;
}




/*列表页*/

.CityShow_2 {
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
}

.CityShow_2 div {
    font-weight: bold;
    text-decoration: none;
    color: #333333;
}

.CityShow_2 img {
    width: 18%;
    margin-left: 12%;
    margin-top: 4%;
}
</style>
