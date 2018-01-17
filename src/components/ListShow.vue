<template>
    <div class="ListMain">
        <div class="top_fun_ara" style="z-index:4;margin-top:-50px;" v-show="!vTopShow">
            <div class="top_func">
                <city v-bind:cityModel="false"></city>
                <input placeholder="输入城市/拼音查询" v-model="SearchBar" @keyup.13="saveKeyword">
                <img class="search_img" src="../../static/obj_img/home_icon_search.png" @click="saveKeyword">
            </div>
            <div class="top_func_2">
                <search v-bind:conditionStyle="false"></search>
            </div>
        </div>
        <div :class="changeUpOrDown(vTopShow)">
            <div class="top_func">
                <city v-bind:cityModel="false"></city>
                <input placeholder="输入城市/拼音查询" v-model="SearchBar" @keyup.13="saveKeyword">
                <img class="search_img" src="../../static/obj_img/home_icon_search.png" @click="saveKeyword">
            </div>
            <div class="top_func_2">
                <search v-bind:conditionStyle="false"></search>
            </div>
        </div>
        <div class="ListArea_inbox">
            <areabox :key="listArr[index].id" v-for="(item, index) in listArr" :message='listArr[index]'></areabox>
        </div>
        <div v-show="false">没有找到活动哦，不过我们会尽快赶来</div>
        <div class="slTime_inbox">
            <articalbox :key="DialtArr[index].id" v-for="(item, index) in DialtArr" :message='DialtArr[index]'></articalbox>
        </div>
        <div id="listBtom_bk"></div>
        <div id="listBottom" style="border-top:1px soild #eee">
            <div v-show="AtTop">
                <img src="../../static/obj_img/tab_list_selected_icon.png">
                <div style="color:#ff3f5c">活动</div>
            </div>
            <div v-show="!AtTop" @click="getTop()">
                <img src="../../static/obj_img/tab_top_icon.png">
                <div style="color:#ff3f5c">回到顶部</div>
            </div>
            <div @click="routerGo('/listCom')">
                <img src="../../static/obj_img/tab_shop_normal_icon.png">
                <div>商城</div>
            </div>
            <div @click="routerGo('/ac')">
                <img src="../../static/obj_img/tab_more_nomal_icon.png">
                <div>我的</div>
            </div>
            <!-- <div @click="routerGo('/mp')">
                <img src="../../static/obj_img/tab_more_normal_icon.png">
                <div>更多</div>
            </div> -->
        </div>
    </div>
</template>

<script>
import City_ from "./ForIndex/city.vue";
import ActiveBox from "./ForList/ListBarStyle_1.vue";
import ActOveBox from "./ForList/ListBarStyle_2.vue";
import ArtialBox from "./ForList/ListBarStyle_3.vue";
import Search_ from "./ForIndex/SearchConditions.vue";
export default {
  data() {
    return {
      data: "",
      SearchBar: this.$store.state.SearchKeyWord || "",
      ListConfig: {
        city_code: this.$store.state.SelectCityCode,
        p: this.$store.state.locationIN.latitude
          ? this.$store.state.locationIN.latitude +
            "," +
            this.$store.state.locationIN.longitude
          : "",
        d:
          parseInt(
            this.$store.state.SelectNearSortCount[0].arrList[
              this.$store.state.SelectNearSortCount[0].sOn
            ]
          ) || this.$store.state.locationIN.latitude
            ? 2000
            : "",
        stext: this.$store.state.SearchKeyWord,
        start: this.YYYYMMdd(
          this.$store.state.ChangeCalendar.startTime || new Date()
        ),
        end: this.YYYYMMdd(
          this.$store.state.ChangeCalendar.endTime ||
            new Date(new Date().getTime() + parseInt(7 * 24 * 60 * 60 * 1000))
        ),
        num:
          this.$store.state.SelectNearSortCount[2].arrList[
            this.$store.state.SelectNearSortCount[2].sOn
          ] == "more"
            ? 10
            : this.$store.state.SelectNearSortCount[2].arrList[
                this.$store.state.SelectNearSortCount[2].sOn
              ],
        page_no: 1,
        page_size: 5,
        sort:
          this.$store.state.SelectNearSortCount[1].sOn == 1
            ? "closing_time|0"
            : "distance|0"
      },
      listArr: [],
      DialtArr: [],
      pageRoll: true,
      vTopShow: true,
      rollScrollCan: true,
      AtTop: window.scrollY < 50
    };
  },
  created() {
    this.getListMaster();
    //this.getListDialog();
  },
  methods: {
    changeUpOrDown(state) {
      return state
        ? "top_fun_ara animated fadeInDown"
        : "top_fun_ara animated fadeOutUp";
    },
    getListMaster(statue) {
      this.$store.commit("ConectionPara", this.ListConfig);
      let getString = this.$store.state.getParamString;
      let postString = getString.replace("?", "");
      this.$http
        .get(this.$store.state.serverHost + "/yqhbsp/event/search" + getString)
        .then(
          statue
            ? m => this.setPaberList_b(m.data)
            : m => this.setPaberList_a(m.data)
        )
        .catch(r => console.log(r));
    },
    getTop() {
      window.scrollTo(0, 0);
    },
    getListDialog() {
      let dialogList = {
        cat_id: 1,
        page_no: 1,
        page_size: 20
      };
      this.$store.commit("ConectionPara", dialogList);
      let getString = this.$store.state.getParamString;
      let postString = getString.replace("?", "");
      this.$http
        .get(this.$store.state.serverHost + "/yqhbsp/article/index" + getString)
        .then(m => this.setDialogList_a(m.data))
        .catch(r => console.log(r));
    },
    YYYYMMdd(data) {
      let ed = new Date(data);
      var yyyy = ed.getFullYear().toString();
      var MM = this.pad(ed.getMonth() + 1, 2);
      var dd = this.pad(ed.getDate(), 2);
      return yyyy + "-" + MM + "-" + dd;
    },
    pad(number, length) {
      var str = "" + number;
      while (str.length < length) {
        str = "0" + str;
      }
      return str;
    },
    setPaberList_a(data) {
      if (data.errcode === 200) {
        this.listArr = this.listArr.concat(data.records);
      } else {
        this.$toast.bottom(data.errmsg);
      }
    },
    setPaberList_b(data) {
      if (data.errcode === 200) {
        this.listArr = data.records;
        //this.listArr = this.pageRoll ? data.records : this.listArr.concat(data.records);
        data.total > 5 ? (this.pageRoll = true) : "";
      } else {
        this.$toast.bottom(data.errmsg);
      }
    },
    setDialogList_a(data) {
      if (data.errcode === 200) {
        this.DialtArr = this.DialtArr.concat(data.records);
      } else {
        this.$toast.bottom(data.errmsg);
      }
    },
    routerGo: function(paths) {
      this.$router.push({ path: paths });
    },
    routerGoBack() {
      this.$router.go(-1);
    },
    saveKeyword() {
      this.ListConfig.stext = this.SearchBar;
      this.$store.commit("saveListKeyWord", this.SearchBar);
    }
  },
  computed: {
    getConfigForList() {
      let newListConfig = {
        city_code: this.$store.state.SelectCityCode,
        p: this.$store.state.locationIN.latitude
          ? this.$store.state.locationIN.latitude +
            "," +
            this.$store.state.locationIN.longitude
          : "",
        d:
          parseInt(
            this.$store.state.SelectNearSortCount[0].arrList[
              this.$store.state.SelectNearSortCount[0].sOn
            ]
          ) || this.$store.state.locationIN.latitude
            ? 2000
            : "",
        stext: this.$store.state.SearchKeyWord,
        start: this.YYYYMMdd(
          this.$store.state.ChangeCalendar.startTime || new Date()
        ),
        end: this.YYYYMMdd(
          this.$store.state.ChangeCalendar.endTime ||
            new Date(new Date().getTime() + parseInt(7 * 24 * 60 * 60 * 1000))
        ),
        num:
          this.$store.state.SelectNearSortCount[2].arrList[
            this.$store.state.SelectNearSortCount[2].sOn
          ] == "more"
            ? 10
            : this.$store.state.SelectNearSortCount[2].arrList[
                this.$store.state.SelectNearSortCount[2].sOn
              ],
        page_no: 1,
        page_size: 10,
        sort:
          this.$store.state.SelectNearSortCount[1].sOn == 1
            ? "closing_time|0"
            : "distance|0"
      };
      return newListConfig;
    }
  },
  watch: {
    getConfigForList(val) {
      this.ListConfig = val;
    },
    ListConfig: {
      handler: function(val, oldVal) {
        val.page_no == 1 ? window.scrollTo(0, 0) : "";
        this.getListMaster(val.page_no == 1);
      },
      deep: true
    },
    rollScrollCan: {
      handler: function(val, oldVal) {
        if (this.rollScrollCan == false) {
          let _this = this;
          setTimeout(function() {
            _this.rollScrollCan = true;
          }, 300);
        }
      },
      deep: true
    }
  },
  components: {
    city: City_,
    search: Search_,
    areabox: ActiveBox,
    articalbox: ArtialBox,
    actovlbox: ActOveBox
  },
  mounted() {
    let _this = this;
    let _scroll = 0;
    let _runCan = true;
    let runArray = [];

    // 设置一个开关来避免重负请求数据
    window.addEventListener("scroll", function() {
      if (
        document.body.scrollTop + window.innerHeight >=
        document.body.offsetHeight
      ) {
        if (_this.pageRoll == true) {
          // 将开关关闭
          _this.pageRoll = false;
          _this.ListConfig.page_no++;
        }
      }
      (window.scrollY - _scroll > 10 || window.scrollY - _scroll < -10) &&
      _this.rollScrollCan
        ? runArray.push(window.scrollY - _scroll)
        : "";
      runArray.length > 3 ? runArray.shift() : "";
      if (runArray[0] > 0 && runArray[1] > 0 && runArray[2] > 0) {
        _this.vTopShow = false;
        _this.rollScrollCan = false;
      }
      if (runArray[0] < 0 && runArray[1] < 0 && runArray[2] < 0) {
        _this.vTopShow = true;
        _this.rollScrollCan = false;
      }
      _this.AtTop = window.scrollY < 50;
      window.scrollY == 0 && _this.rollScrollCan ? (_this.vTopShow = true) : "";
      _scroll = window.scrollY;
    });
    let share = {
      sharetitle: "我喜欢这个绘画活动，你也来一起画吧",
      sharelink:"https://wx.17hua.me/#/?u_id=" +this.$store.state.usrInfomation.user_id,
      shareimgUrl:"https://image.17hua.me/upload/image/201709/zoud6drye2zobx7jggu629a4vrn2yttw.png",
      shareDesc: "创建独一无二的作品！"
    };
    this.$store.commit("useShareFun", share);
  }
};
</script>

<style>
body {
  background-color: #f5f5f5;
  text-decoration: none;
}

.ListMain .ListArea_inbox {
  width: 100%;
  float: left;
}

.ListArea_inbox {
  margin-top: 91px;
}

.ListMain {
  margin-bottom: 60px;
}

.slTime_inbox {
  -webkit-column-count: 2;
  column-count: 2;
  width: 94.2%;
  padding: 2.9%;
}

.top_fun_ara {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  z-index: 5;
  background-color: #ffffff;
  border-bottom: 1px solid #f5f5f5;
}

.top_func {
  float: left;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  width: 94.2%;
  padding: 0 2.9%;
  height: 50px;
  background-color: #ffffff;
}

.top_func_2 {
  float: left;
  border-top: 1px solid #eeeeee;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  width: 100%;
  height: 40px;
  background-color: #ffffff;
}

.top_func input {
  flex: 1;
  -webkit-flex: 1;
  -webkit-border-top-left-radius: 50px;
  -webkit-border-bottom-left-radius: 50px;
  font-size: 16px;
  border: 0;
  margin: 0 -1px 0 0;
  padding: 0 0 0 3.4%;
  background-color: #f2f2f2;
  height: 30px;
  line-height: 30px;
  outline: none;
}

.search_img {
  -webkit-border-top-right-radius: 50px;
  -webkit-border-bottom-right-radius: 50px;
  font-size: 16px;
  border: 0;
  background-color: #f2f2f2;
  width: 20px;
  height: 20px;
  padding: 5px 3.4%;
}

#listBtom_bk {
  position: fixed;
  left: 0;
  bottom: 0;
  height: 45px;
  z-index: 1;
  background: hsla(0, 0%, 100%, 0.3);
  -webkit-filter: blur(2px);
  filter: blur(2px);
  background: rgba(255, 255, 255, 0.7);
  width: 100%;
  z-index: 100;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  justify-content: space-between;
  -webkit-justify-content: space-between;
}

#listBottom {
  position: fixed;
  z-index: 3;
  left: 0;
  bottom: 0;
  width: 100%;
  background: hsla(0, 0%, 100%, 0.9);
  z-index: 100;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  justify-content: space-between;
  -webkit-justify-content: space-between;
}

#listBottom div {
  padding: 5px 5% 0 5%;
  width: 50px;
  display: flex;
  display: -webkit-flex;
  flex-direction: column;
  -webkit-flex-direction: column;
  align-items: center;
  -webkit-align-items: center;
}

#listBottom div div {
  -webkit-transform: scale(0.8);
  font-size: 12px;
}

#listBottom img {
  width: 24px;
  height: 24px;
  margin-bottom: -5px;
}
</style>
