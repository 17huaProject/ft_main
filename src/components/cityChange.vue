<template>
  <div class="cityArea">
    <div class="localCity">
      <img src="../../static/obj_img/search_icon_local.png">
      <div>{{city}}
        <span>GPS定位</span>
      </div>
    </div>
    <div class="selectArea">
      <div :key="item.name" v-for="(item, index) in cityArr" v-on:click="clickThisCity(index)">{{ item.name }}</div>
    </div>
  </div>
</template>

<script>

import CenterTrunk from '../assets/eventBus'
export default {
  data() {
    return {
      city: '上海',
      cityArr: this.$store.state.SelectCityArray,
      getTsr: new Date(),
    }
  },
  created() {
    // 组件创建完后获取数据，
    // 此时 data 已经被 observed 了
    this.getCityArr()
  },
  watch: {
    // '$route': 'getCityArr'
  },
  methods: {
    clickThisCity(index) {
      this.$store.commit('changeCity', { name: this.cityArr[index].name, code: this.cityArr[index].code });
      this.$router.go(-1)
    },
    getCityArr() {
      if (this.$store.state.SelectCityArray.length === 0) {
        this.$store.commit('ConectionPara', {})
        let getString = this.$store.state.getParamString;
        let postString = getString.replace('?', '');
        this.$http.get(this.$store.state.serverHost + '/yqhbsp/config/supportCity' + getString).then(m => this.setCityArray(m.data)).catch(r => console.log(r));
      }
    },
    setCityArray(data) {
      if (data.errcode == 200) {
        this.cityArr = data.records;
        this.$store.commit('getCityArray', data.records)
      } else {
        this.$toast.center(data.errmsg)
      }
    }
  },
  mounted() {

  }
}
</script>

<style>
body {
  background-color: #f5f5f5
}

router-link {
  width: 100%;
}

.cityArea {
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  flex-direction: column;
  -webkit-flex-direction: column;
  width: 100%
}

.localCity {
  padding: 4.3% 0;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  width: 100%;
  justify-content: center;
  -webkit-justify-content: center;
}

.localCity img {
  width: 3.3%
}

.localCity div {
  font-size: 16px;
  margin-left: 1.4%;
  color: #333333;
}


.localCity div span {
  font-size: 12px;
  color: #666666;
}


.selectArea {
  width: 100%;
  flex: 1;
  -webkit-flex: 1;
  float: left;
}

.selectArea div {
  width: 29%;
  padding: 4.3% 0;
  margin-left: 3.2%;
  margin-bottom: 2.1%;
  background-color: #ffffff;
  color: #333333;
  text-align: center;
  float: left;
  font-size: 14px;
}

.selectArea div:active {
  width: 29%;
  padding: 4.3% 0;
  margin-left: 3.2%;
  margin-bottom: 2.1%;
  background-color: #f5f5f5;
  color: #333333;
  text-align: center;
  float: left;
  font-size: 14px;
}
</style>
