<template>
    <div class="search">
        <div class="search_input">
            <div class="search_box">
                <div></div>
                <input v-model="SearchBar" @keyup.13="pushArray" placeholder="开始一起画">
                <img @click="searchList" src="../../../static/obj_img/home_icon_search.png">
            </div>
        </div>
        <div class="search_tips" v-show="false">
            <div class="triangle_border_up triangle_border_up_local1"></div>
            <div class="tips_line">
                <div :key="item" v-for="(item,index) in ttipsInfo.arrList">{{item}}</div>
            </div>
        </div>
        <div class="search_conditions">
            <searchConditions v-bind:conditionStyle="true"></searchConditions>
        </div>
    </div>
</template>

<script>
import searchConditions from './SearchConditions.vue'
export default {
    name: 'search',
    data() {
        return {
            SearchBar:this.$store.state.SearchKeyWord ||'',
            ttipsInfo:this.$store.state.SelectNearSortCount.arrList||{},
            date: '',
            calshow: true,
        }
    },
    methods: {
        pushArray: function (ev) {
            if (ev.keyCode == 13) {
                this.$store.commit('saveListKeyWord', this.SearchBar)
                this.$router.push({ path: '/list' })
            }
        },
        searchList() {
            this.$router.push({ path: '/list' })
        },
        sOn(index) {
            return index == 0 ? 'color_sl List_tips' : 'List_tips'
        },
    },
    beforeUpdate() {

    },
    watch: {
        ttipsInfo(){
            return this.$store.state.SelectNearSortCount || ""
        }
    },
    mounted() {

    },
    components: { searchConditions }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.List_tips {
  font-size: 16px;
  width: 100%;
  text-align: center;
  padding: 5px 0 ;
}
.List_tips:active {
  background-color: #f5f5f5;
  color: #666;
}
.color_em {
  background-color: #ffffff;
  color: #999;
}
.color_sl {
  background-color: #f5f5f5;
  color: #666;
}
/*搜索框*/

.search_input {
    position: absolute;
    left: 0;
    top: 0;
    margin-top: 67.1%;
    width: 100%;
    display: flex;
    display: -webkit-flex;
}

.search_box {
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    -webkit-justify-content: center;
    margin: 0 9%;
    color: #666;
    flex: 1;
    -webkit-flex: 1;
    background-color: #ffffff;
    -webkit-border-radius: 100px;
    border-radius: 100px;
}

.search_box div {
    width: 1%;
    padding: 4.7% 0 4.7% 6.3%;
}

.search_box input {
    flex: 1;
    border: 0;
    outline: none;
    font-size: 16px;
    padding: 0;
}

.search_box img {
    width: 6.7%;
    height: 40.4%;
    margin: 4.7% 6.3% 4.7% 6.3%;
}








/*搜索标签*/

.search_tips {
    position: absolute;
    left: 0;
    top: 0;
    z-index: 2;
    margin: 84.5% 14% 0 14%;
    width: 72%;
    background-color: #ffffff;
    -webkit-border-radius: 5px;
    border-radius: 5px;
}

.guest_tips {
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;
    margin: 97% 14% 0 14%;
    width: 27%;
    background-color: #ffffff;
    -webkit-border-radius: 5px;
    border-radius: 5px;
}

.triangle_border_up {
    width: 0;
    height: 0;
    border-width: 0 9px 11px;
    border-style: solid;
    border-color: transparent transparent #fff;
    /*透明 透明  灰*/
    position: relative;
}

.triangle_border_up_local1 {
    margin: -10px 0 0 16.8%;
}

.triangle_border_up_local2 {
    margin: -10px 0 0 40%;
}

.triangle_border_up_local3 {}

.tips_line {
    width: 100%;
    padding: 12px 0;
    text-align: left;
    -webkit-animation: downShow 0.3s;
}

@-webkit-keyframes downShow {
    from {
        margin-top: -10px
    }
    to {
        margin: 0
    }
}

.blank_close {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
}





/*搜索条件*/

.search_conditions {
    position: absolute;
    left: 0;
    top: 0;
    margin-top: 85.8%;
    width: 100%;
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    -webkit-justify-content: center;
}

.calendar_area {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
}
</style>