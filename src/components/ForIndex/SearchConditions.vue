<template>
    <div style="width:100%">
        <div v-show="modelNum" class="search_conditions_box">
            <!--第一套-->
            <div class="calender_setTime" @click="selectCalendar()">
                <div class="select_day">{{today.day}}日</div>
                <div class="select_mtwk">
                    <div>{{today.month}}月</div>
                    <div>周{{today.week}}</div>
                </div>
            </div>
            <img class="select_arr" src="../../../static/obj_img/home_icon_ArrRight.png">
            <div class="calender_setTime" @click="selectCalendar()">
                <div class="select_day">{{afterweek.day}}日</div>
                <div class="select_mtwk">
                    <div>{{afterweek.month}}月</div>
                    <div>周{{afterweek.week}}</div>
                </div>
            </div>
            <div class="consLine"></div>
            <div class="condition_peoples" @click="selectCount(0,2)">{{guest}}人参加</div>
            <img class="select_arr" style="margin-top:1.2%" src="../../../static/obj_img/home_icon_ArrDown.png">
    
        </div>
        <!-- tips -->
        <div class="bk_lock" v-show="tipsShow[0]||tipsShow[1]||tipsShow[2]||tipsShow[3]" @click="closeAll"></div>
        <div class="guest_tips_0" v-show="tipsShow[0]">
            <div class="triangle_border_up triangle_border_up_local0"></div>
            <div class="tips_line">
                <div :class="sOn(index)" :key="item" v-for="(item,index) in ttipsInfo.arrList" @click="SelectThis(2,index)">{{item}}</div>
            </div>
        </div>
        <div class="guest_tips_1" v-show="tipsShow[1]">
            <div class="triangle_border_up triangle_border_up_local1"></div>
            <div class="tips_line">
                <div :class="sOn(index)" :key="item" v-for="(item,index) in ttipsInfo.arrList" @click="SelectThis(0,index)">{{item}}</div>
            </div>
        </div>
        <div class="guest_tips_2" v-show="tipsShow[2]">
            <div class="triangle_border_up triangle_border_up_local2"></div>
            <div class="tips_line">
                <div :class="sOn(index)" :key="item" v-for="(item,index) in ttipsInfo.arrList" @click="SelectThis(1,index)">{{item}}</div>
            </div>
        </div>
        <div class="guest_tips_3" v-show="tipsShow[3]">
            <div class="triangle_border_up triangle_border_up_local3"></div>
            <div class="tips_line">
                <div :class="sOn(index)" :key="item" v-for="(item,index) in ttipsInfo.arrList" @click="SelectThis(2,index)">{{item}}</div>
            </div>
        </div>
        <div v-show="!modelNum" class="search_conditions_box_2">
            <!--第二套-->
            <div class="condition_peoples_21" @click="selectCount(1,0)">{{this.$store.state.SelectNearSortCount[0].arrList[this.$store.state.SelectNearSortCount[0].sOn]}}
                <img class="condition_peoples_2_img" src="../../../static/obj_img/list_img/list_seratch_icon_1.png">
            </div>
            <div class="condition_peoples_sting"></div>
            <div class="condition_peoples_2" v-show="false" @click="selectCalendar()">{{today.month}}.{{today.day}}-{{afterweek.month}}.{{afterweek.day}}</div>
            <div class="condition_peoples_23" @click="selectCount(2,1)">{{this.$store.state.SelectNearSortCount[1].arrList[this.$store.state.SelectNearSortCount[1].sOn]}}
                <img class="condition_peoples_2_img" src="../../../static/obj_img/list_img/list_seratch_icon_1.png">
            </div>
             <div class="condition_peoples_sting"></div>
            <div class="condition_peoples_24" @click="selectCount(3,2)">{{this.$store.state.SelectNearSortCount[2].arrList[this.$store.state.SelectNearSortCount[2].sOn] == 'more' ? '多' : this.$store.state.SelectNearSortCount[2].arrList[this.$store.state.SelectNearSortCount[2].sOn]}}人
                <img class="condition_peoples_2_img" src="../../../static/obj_img/list_img/list_seratch_icon_1.png">
            </div>
        </div>
    </div>
</template>

<script>
// const toDay = new Date();
// const afterWeek = new Date(toDay.getTime() + parseInt(7 * 24 * 60 * 60 * 1000));
const weekName = ['日', '一', '二', '三', '四', '五', '六'];

export default {
    data() {
        return {
            tipsShow: [false, false, false, false],
            today: {//当前时间
                year: this.$store.state.ChangeCalendar.startTime.getFullYear(),
                day: this.$store.state.ChangeCalendar.startTime.getDate() < 10 ? '0' + this.$store.state.ChangeCalendar.startTime.getDate() : this.$store.state.ChangeCalendar.startTime.getDate(),
                month: (this.$store.state.ChangeCalendar.startTime.getMonth() + 1) < 10 ? '0' + (this.$store.state.ChangeCalendar.startTime.getMonth() + 1) : (this.$store.state.ChangeCalendar.startTime.getMonth() + 1),
                week: weekName[this.$store.state.ChangeCalendar.startTime.getDay()],
            },
            afterweek: {//一周之后
                year: this.$store.state.ChangeCalendar.endTime.getFullYear(),
                day: this.$store.state.ChangeCalendar.endTime.getDate() < 10 ? '0' + this.$store.state.ChangeCalendar.endTime.getDate() : this.$store.state.ChangeCalendar.endTime.getDate(),
                month: (this.$store.state.ChangeCalendar.endTime.getMonth() + 1) < 10 ? '0' + (this.$store.state.ChangeCalendar.endTime.getMonth() + 1) : (this.$store.state.ChangeCalendar.endTime.getMonth() + 1),
                week: weekName[this.$store.state.ChangeCalendar.endTime.getDay()],
            },
            guest: this.$store.state.SelectNearSortCount[2].arrList[this.$store.state.SelectNearSortCount[2].sOn] == 'more' ? '多' : this.$store.state.SelectNearSortCount[2].arrList[this.$store.state.SelectNearSortCount[2].sOn],
            SELECT: 0,
            WITIME_SELECT: 0,
            toDayorafTer: '',
            modelNum: this.conditionStyle,
            ttipsInfo: {},
        }
    },
    computed: {

    },
    props: ["conditionStyle"],
    methods: {
        selectCalendar: function (params) {
            let needChangeTime = (this.today.year + '-' + this.today.month + '-' + this.today.day) + ',' + (this.afterweek.year + '-' + this.afterweek.month + '-' + this.afterweek.day)
            this.$router.push({ name: 'cladr', query: { slCla: needChangeTime } })
        },
        selectCount: function (local, list) {
            this.tipsShow = [false, false, false, false]
            this.tipsShow[local] = true;
            this.ttipsInfo = this.$store.state.SelectNearSortCount[list]
        },
        sOn(index) {
            return index === this.ttipsInfo.sOn ? 'color_sl List_tips' : 'List_tips'
        },
        SelectThis(list, index) {
            let c = {
                index: list,
                num: index
            }
            this.$store.commit('setSelectNearSortCount', c)
            this.guest = this.$store.state.SelectNearSortCount[2].arrList[this.$store.state.SelectNearSortCount[2].sOn] == 'more' ? '多' : this.$store.state.SelectNearSortCount[2].arrList[this.$store.state.SelectNearSortCount[2].sOn]
            this.tipsShow = [false, false, false, false]
        },
        closeAll() {
            this.tipsShow = [false, false, false, false]
        }
    },
    mounted() {
        
    }
}
</script>

<style>
.search_conditions_box {
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
    color: #ffffff;
}

.calender_setTime {
    display: flex;
    display: -webkit-flex;
}

.select_day {
    font-size: 20px;
}

.select_mtwk {
    transform: scale(0.8);
    -webkit-transform: scale(0.8);
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    margin: -2.2% 0 0 0;
}

.select_mtwk div:first-child {
    font-size: 12px;
}

.select_mtwk div:last-child {
    font-size: 12px;
    color: #b6b6b6;
}

.select_arr {
    width: 7.9%;
}

.consLine {
    width: 1px;
    height: 20px;
    background-color: #eeeeee;
    margin: 0 4.6%;
}

.condition_peoples {
    font-size: 16px;
    margin-right: 1.3%;
}












/*secoundStyle*/

.search_conditions_box_2 {
    width: 94.2%;
    margin: 0 2.9%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: space-between;
    -webkit-justify-content: space-between;
    color: #333333;
}

.calender_setTime_2 {
    display: flex;
    display: -webkit-flex;
    flex: 1;
    -webkit-flex: 1;
    justify-content: center;
    -webkit-justify-content: center;
}

.select_day_2 {
    margin-top: 3px;
    font-size: 18px;
}

.select_mtwk_2 {
    transform: scale(0.7);
    -webkit-transform: scale(0.7);
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
    margin: -3% 0 0 0;
}

.select_mtwk_2 div:first-child {
    font-size: 12px;
}

.select_mtwk_2 div:last-child {

    font-size: 12px;
    color: #333333;
}


.select_arr_22 {
    width: 5.6%;
}

.consLine_2 {
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    border-left: 1px solid #969696;
    border-right: 1px solid #969696;
    margin: 0 3.7%;
    padding: 0 3.7%;
}

.condition_peoples_2 {
    font-size: 14px;
    flex: 1;
    -webkit-flex: 1;
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    -webkit-justify-content: center;
    align-items: center;
    -webkit-align-items: center;
    text-align: center;
}

.condition_peoples_2_img {
    margin-left: 5%;
    margin-top: 3px;
    height: 10px;
}

.condition_peoples_21 {
    flex:  0.5;
    -webkit-flex:  0.5;
    font-size: 14px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
}

.condition_peoples_23 {
    flex: 1;
    -webkit-flex: 1;
    font-size: 14px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
}

.condition_peoples_24 {
    flex: 0.5;
    -webkit-flex:  0.5;
    font-size: 14px;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    -webkit-align-items: center;
    justify-content: center;
    -webkit-justify-content: center;
}
.condition_peoples_sting {
    width: 1px;
    height: 20px;
    background-color: #f1f1f1
}




/* tip_area */

.guest_tips_0 {
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;
    margin: 11% 14% 0 14%;
    width: 27%;
    background-color: #ffffff;
    -webkit-border-radius: 5px;
    border-radius: 5px;
}

.guest_tips_1 {
    position: absolute;
    left: 0;
    top: 0;
    z-index: 10;
    margin: 105px 0 0 2%;
    width: 27%;
    background-color: #ffffff;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    border: 1px solid #f5f5f5;
}

.guest_tips_2 {
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;
    margin: 105px 36.5% 0 0;
    width: 27%;
    background-color: #ffffff;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    border: 1px solid #f5f5f5;
}

.guest_tips_3 {
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;
    margin: 105px 2% 0 0;
    width: 27%;
    background-color: #ffffff;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    border: 1px solid #f5f5f5;
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


.triangle_border_up_local0 {
    margin: -10px 0 0 40%;
}

.triangle_border_up_local1 {
    margin: -10px 0 0 15%;
}

.triangle_border_up_local2 {
    margin: -10px 0 0 40%;
}

.triangle_border_up_local3 {
    margin: -10px 0 0 65%;
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

.List_tips {
    font-size: 16px;
    width: 100%;
    text-align: center;
    padding: 5px 0;
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

.bk_lock {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    opacity: .2;
    background-color: #f1f1f1;
}
</style>