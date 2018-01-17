<template>
  <div id="tipsline">
    <div class="bk_close" v-show="false" ></div>
    <div :key="item" v-for="(item, index) in TipsInfo.arrList" :class="sOn(index)">{{item}} 人参加</div>
  </div>
</template>

<script>
var Changex = 0;
export default {
  data() {
    return {
      TipsInfo:{arrList:[123,123]}
    }
  },
  methods: {
     sOn(index){
       return index==0?'color_sl List_tips':'List_tips'
     }
  },
  watch:{
  
    
  },
  computed:{
 
  },
  props:['message']
  ,
  mounted(){
    
  }
}
</script>

<style>
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

.tecen {
  text-align: center
}

.bk_close {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
}
</style>
