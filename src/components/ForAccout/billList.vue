<template>
    <div id="tpa">
        <!-- <div class="tpa_table">
            <div class="tpa_class">
                可使用画贝<span style="color:#ff3f5c">2</span>
            </div>
            <div class="tpa_line"></div>
            <div class="tpa_class">
                成功邀请<span>2人</span>
            </div>
        </div> -->
        <div style="width:100%;text-align:center;padding:5px 0">- 画贝消费明细 -</div>
        <div class="Tline" :key="item" v-for="(item, index) in billList">
            <div class="maLst">
                <div>{{item.description}}</div>
                <div>{{item.create_time}}</div>
            </div>
            <div>{{item.money}}</div>
        </div>
        <div class="empty_list" v-if="billList.length==0">
            <img src="../../../static/obj_img/accout_img/huabei_list_normal_icon.png">
            <div>快去邀请你的好友下单吧~</div>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      billList: [],
      Winfo: {},
      page_no: 1,
      pageRoll: true
    };
  },
  methods: {
    reAd(statue) {
      return statue ? "+" : "-";
    },
    getNumberSis(data) {
      if (data.errcode === 200) {
        data.records.length<20?this.pageRoll=false:''
        this.billList = data.records;
      } else {
        this.$toast.center(data.errmsg);
      }
    },
    getListSis(data) {
      if (data.errcode === 200) {
        this.Winfo = data.record;
      } else {
        this.$toast.center(data.errmsg);
      }
    },
    getListSisAr(data) {
      if (data.errcode === 200) {
        data.records.length>20?this.pageRoll=true:''
        this.billList = this.billList.concat(data.records);
      } else {
        this.$toast.center(data.errmsg);
      }
    },
    getListMaster(num) {
      let a = {
        access_token: this.$store.state.usrInfomation.access_token,
        page_no: num,
        page_size: 20,
        type: 0
      };
      this.$store.commit("ConectionPara", a);
      let getString = this.$store.state.getParamString;
      let postString = getString.replace("?", "");
      this.$http.get(this.$store.state.serverHost +"/yqhbsp/account/showMoneyDetail" +getString).then(m => this.getListSisAr(m.data)).catch(r => console.log(r));
    }
  },
  created() {
    let a = {
      access_token: this.$store.state.usrInfomation.access_token,
      page_no: 1,
      page_size: 20,
      type: 0
    };
    this.$store.commit("ConectionPara", a);
    let getString = this.$store.state.getParamString;
    let postString = getString.replace("?", "");
    this.$http
      .get(
        this.$store.state.serverHost +
          "/yqhbsp/account/showMoneyDetail" +
          getString
      )
      .then(m => this.getNumberSis(m.data))
      .catch(r => console.log(r));
  },
  watch: {
    page_no:{
      handler: function(val, oldVal) {
        this.getListMaster(val);
      },
      deep: true
    }
  },
  mounted() {
    let _this = this;
    window.addEventListener("scroll", function() {
      if (
        document.body.scrollTop + window.innerHeight >=
        document.body.offsetHeight
      ) {
        if (_this.pageRoll == true) {
          _this.pageRoll = false;
          _this.page_no++;
        }
      }
    });
  }
};
</script>

<style>
body {
  background-color: #f5f5f5;
}

#tpa {
  width: 100%;
  float: left;
}

.Tline {
  width: 93.4%;
  padding: 3.3%;
  float: left;
  background-color: #ffffff;
  margin-bottom: 1%;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  color: #333333;
  font-size: 16px;
}

.maLst {
  flex: 1;
  -webkit-flex: 1;
}

.maLst div:last-child {
  color: #999999;
  margin-top: 3.3%;
}
.empty_list {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.empty_list img {
  height: 120px;
}
.empty_list div {
  font-size: 14px;
  margin: 10px 0;
}

.tpa_table {
  width: 100%;
  margin: 10px 0;

  background-color: #fff;
  display: flex;
}
.tpa_class {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 14px;
  padding: 20px 0;
  color: #333;
}
.tpa_class span {
  padding: 5px 0;
  font-size: 16px;
}
.tpa_line {
  width: 2px;
  background-color: #f5f5f5;
}
</style>
