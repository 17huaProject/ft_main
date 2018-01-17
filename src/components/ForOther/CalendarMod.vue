<template>
    <div class="calendarMore">
        <div class="each_month" :key="pindex" v-for="(item,pindex) in crtMot">
            <div class="monthArae">{{crtMot[pindex][0]}}</div>
            <div class="weekArea">
                <div>日</div>
                <div>一</div>
                <div>二</div>
                <div>三</div>
                <div>四</div>
                <div>五</div>
                <div>六</div>
            </div>
            <div class="dayArea">
                <div :key="item" v-for="(item,index) in crtMot[pindex][2]"></div>
                <div :key="item" v-for="(item,index) in crtMot[pindex][1]" :class="classBind(index+1,crtMot[pindex])" @click="selctOnChange(crtMot[pindex][0],(index+1))">{{index+1}}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            crtMot: [],
            runD: '',
            mesDaty: 23,
            changeTime: this.$route.query.slCla,
            is_single: this.$route.query.single || false,
            sle1: '',
            sle2: ''
        }
    },
    created() {
        this.ruM()
    },
    methods: {
        ruM() {
            let thisM = new Date();
            let YM = thisM.getFullYear() + '-' + (thisM.getMonth() + 1 < 10 ? '0' + (thisM.getMonth() + 1) : thisM.getMonth() + 1)//获取当月
            let AllDay = new Date(thisM.getFullYear(), (thisM.getMonth() + 1), 0).getDate();//获取一个月多少天
            let weeOn = new Date(thisM.setDate(1)).getDay()//获取第一天是周几
            let today = new Date().getDate()//获取今天是
            this.crtMot.push([YM, AllDay, weeOn, today]);
            this.runD = YM + '-' + 1
            this.ruN(this.runD, this.mesDaty)
        },
        classBind(d, t) {
            let Y = t[0].replace('-', '/') + (d < 10 ? '/0' + d : '/' + d);
            let h8s = 8 * 60 * 60 * 1000
            let v1 = new Date(this.changeTime.split(',')[0]).getTime() - h8s, v2 = new Date(this.changeTime.split(',')[1]).getTime();
            let Cle = new Date(Y).getTime()
            return d < t[3] ? 'disSelectDay' : Cle >= v1 && Cle <= v2 ? 'onSelect' : 'cSelect'
        },
        ruN(n, me) {
            let newY = n.split('-')[0]
            let newM = n.split('-')[1];
            if (newM > 11) {
                newM = 1;
                newY++;
            } else {
                newM++
            }
            n = newY + '/' + newM + '/1'
            let thisM = new Date(n);
            let YM = thisM.getFullYear() + '-' + (thisM.getMonth() + 1 < 10 ? '0' + (thisM.getMonth() + 1) : thisM.getMonth() + 1)//获取当月
            let AllDay = new Date(thisM.getFullYear(), (thisM.getMonth() + 1), 0).getDate();//获取一个月多少天
            let weeOn = new Date(thisM.setDate(1)).getDay()//获取第一天是周几
            this.crtMot.push([YM, AllDay, weeOn, 0]);
            this.runD = YM + '-' + 1;
            this.mesDaty--
            return true
        },
        selctOnChange(data, d) {
            let thisM = new Date();
            data = data.replace('-', '/')
            d = d < 10 ? '/0' + d : '/' + d
            data += d
            let thisY = new Date(thisM.getFullYear() + '/' + (thisM.getMonth() + 1) + '/' + thisM.getDate()).getTime();
            let thisT = new Date(data).getTime();
            if ((thisT - thisY) >= 0) {
                if (this.is_single) {
                    let t1 = new Date(thisT).getTime();
                    let _thisTime = new Date(t1).getFullYear() + '-' + (1 + new Date(t1).getMonth()) + '-' + new Date(t1).getDate()
                    this.$store.commit('SelectTime', _thisTime);
                    this.$router.go(-1)
                } else {
                    if (this.sle1 == '' && this.sle2 == '') {
                        this.sle1 = data
                    } else if (this.sle1 != '' && this.sle2 == '') {
                        let t1 = new Date(this.sle1).getTime(), t2 = new Date(data).getTime()
                        if (t1 <= t2) {
                            this.sle2 = data
                            let time = {
                                startTime: new Date(this.sle1),
                                endTime: new Date(this.sle2)
                            }
                            this.$store.commit('ChangeTime', time);
                            this.$router.go(-1)
                        } else {
                            this.sle1 = data;
                        }
                    }
                    this.changeTime = this.sle1 + ',' + (this.sle2 == '' ? this.sle1 : this.sle2)
                }
            }
        }
    },
    watch: {

    },
    computed: {

    },
    mounted() {
        let _this = this;
        // 设置一个开关来避免重负请求数据  
        let sw = true;
        window.addEventListener('scroll', function () {
            if (document.body.scrollTop + window.innerHeight >= document.body.offsetHeight) {
                if (sw == true) {
                    // 将开关关闭  
                    sw = false;
                    sw = _this.mesDaty > 0 ? _this.ruN(_this.runD, _this.mesDaty) : false
                }
            }
        });
    }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

.calendarMore {
    width: 100%;
    float: left;
}

.each_month {
    width: 90%;
    padding: 5%;
    float: left;
    background-color: #ffffff;
    margin-bottom: 1.6%;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: column;
}

.monthArae {
    width: 100%;
    float: left;
    text-align: center;
    margin-bottom: 5%;
}

.weekArea {
    width: 100%;
    float: left;
}

.weekArea div {
    width: 14.28%;
    float: left;
    font-size: 16px;
    color: #999999;
    text-align: center;
    padding-bottom: 3.2%;
}

.weekArea div:first-child,
.weekArea div:last-child {
    color: #ff604f
}

.dayArea {
    width: 100%;
    float: left;
    color: #333333;
}

.dayArea div {
    width: 13.28%;
    float: left;
    text-align: center;
    padding: 3.3% 0;
    margin: 0.5%;
    -webkit-border-radius: 3px;
    border-radius: 3px;
}

.cSelect {
    background-color: #ffffff;
    color: #333333;
}

.onSelect {
    background-color: #ff604f;
    color: #ffffff;
}

.disSelectDay {
    background-color: #f1f1f1;
    color: #333333;
}
</style>
