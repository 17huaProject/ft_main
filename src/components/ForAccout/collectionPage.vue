<template>
    <div class="manListBar_collection">
        <div class="collectionBar">
            <div :class="Select[0]" @click="aleThiss(0)">活动</div>
            <div :class="Select[1]" @click="aleThiss(1)">画师</div>
            <div :class="Select[2]" @click="aleThiss(2)">空间</div>
        </div>
        <div class="listArea_collection">
            <alike v-show="childShow[0]"></alike>
            <plike v-show="childShow[1]"></plike>
            <slike v-show="childShow[2]"></slike>
        </div>
    </div>
</template>

<script>
import alike from './Forcollection/activeLike.vue'
import plike from './Forcollection/painterLike.vue'
import slike from './Forcollection/spaceLike.vue'
export default {
     data() {
    return {
      Select: ['onSelect_c', 'diSelect_c', 'diSelect_c'],
      childShow: [true, false, false],
    }
  },
    methods: {
        aleThiss: function (num) {//切换标签
            this.Select = []; this.childShow = [];
            for (let i = 0; i < 3; i++) {
                i === num ? this.Select.push('onSelect_c') : this.Select.push('diSelect_c')
                i === num ? this.childShow.push(true) : this.childShow.push(false)
            }
        }
    },
    components: { alike, plike, slike }
}
</script>

<style>
body {
    background-color: #f5f5f5;
}

.manListBar_collection,
.listArea_collection {
    float: left;
    width: 100%;
}

.collectionBar {
    display: flex;
    display: -webkit-flex;
    padding: 2% 0;
    background-color: #ffffff;
}

.collectionBar div {
    flex: 1;
    -webkit-flex: 1;
    margin: 0 10%;
    padding: 0 0 2% 0;
    text-align: center;
    font-size: 16px;
}
.onSelect_c{
    color: #ff605f;
    border-bottom: 2px solid #ff605f;
}
.diSelect_c{
    color: #333333;
    border-bottom: 1px solid #ffffff;
}
</style>
