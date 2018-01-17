<template>
  <div id="GiftCardSetArea">
    <div id="title_GiftCard">选择送给朋友的心意贺卡</div>
    <div id="GiftCardBox_textArea" :style="{top:getWith*2/3+'px'}">
      <input maxlength="30" placeholder="To:" v-model="TextAreaInfo.GiftToText">
      <textarea maxlength="60" placeholder="不超过60字" v-model="TextAreaInfo.GiftContext"></textarea>
      <input maxlength="30" placeholder="By:" v-model="TextAreaInfo.GiftByName">
    </div>
    <div class="sweiperArea_box" :style="{height:getWith*5/4+'px'}">
      <swiper :options="swiperOptionTop" class="gallery-top" ref="swiperTop">
        <swiper-slide :style="{backgroundImage: 'url(\''+index+'\')'}" class="ps_swiperSlide_relative" :key="'wbTop'+index" v-for="index in banners">
        </swiper-slide>
      </swiper>
      <!-- swiper2 Thumbs -->
      <swiper :options="swiperOptionThumbs" class="gallery-thumbs" ref="swiperThumbs">
        <swiper-slide :style="{backgroundImage: 'url(\''+index+'\')'}" :key="'wbDown'+index" v-for="index in banners"></swiper-slide>
      </swiper>
    </div>
    <div id="nextStep_GiftCard" @click="NextStepTo">下一步：选择金额</div>
  </div>
</template>

<script>

export default {
  data() {
    return {
       TextAreaInfo: {
        swiperIndex: this.$store.state.GiftCardCustInfo.page || 0,
        GiftToText: this.$store.state.GiftCardCustInfo.to || '',
        GiftContext: this.$store.state.GiftCardCustInfo.context || '',
        GiftByName: this.$store.state.GiftCardCustInfo.by || 'by:'+this.$store.state.usrInfomation.nickname
      },
      banners: ['../../../static/obj_img/giftcard_img/gift_heka_photo_2.png', '../../../static/obj_img/accout_img/my_collect_place.png', '../../../static/obj_img/accout_img/my_collect_place.png', '../../../static/obj_img/accout_img/my_collect_paiter.png', '../../../static/obj_img/accout_img/my_collect_place.png', '../../../static/obj_img/accout_img/my_collect_place.png'],
      swiperOptionTop: {
        notNextTick: true,
        initialSlide: this.$store.state.GiftCardCustInfo.page || 0,
        onSlideChangeEnd: swiper => {
          this.TextAreaInfo.swiperIndex = swiper.realIndex
        }
      },
      swiperOptionThumbs: {
        notNextTick: true,
        spaceBetween: 10,
        centeredSlides: true,
        slidesPerView: 'auto',
        touchRatio: 0.2,
        initialSlide: this.$store.state.GiftCardCustInfo.page || 0,
        slideToClickedSlide: true
      },
      getWith: document.documentElement.clientWidth,
     
    }
  },
  watch: {
    TextAreaInfo: {
      handler: function (val, oldVal) {
        this.saveGiftCardInfo()
      },
      deep: true
    },
  },
  methods: {
    saveGiftCardInfo() {
      let mesSate = {};
      mesSate.page = this.TextAreaInfo.swiperIndex;
      mesSate.to = this.TextAreaInfo.GiftToText;
      mesSate.context = this.TextAreaInfo.GiftContext;
      mesSate.by = this.TextAreaInfo.GiftByName;
      mesSate.num = 0;
      this.$store.commit('saveGiftCardStyleinfo', mesSate)
    },
    NextStepTo(){
      if(this.TextAreaInfo.GiftToText==''){
        this.$toast.bottom('请填写领卡人姓名')
        return false
      }
      if(this.TextAreaInfo.GiftContext==''){
        this.$toast.bottom('不写上祝福语么？')
        return false
      }
      if(this.TextAreaInfo.GiftByName==''){
        this.$toast.bottom('不告诉别人你是谁么')
        return false
      }
      this.$router.push('/gcb')
    }
  },
  mounted() {
    const swiperTop = this.$refs.swiperTop.swiper
    const swiperThumbs = this.$refs.swiperThumbs.swiper
    swiperTop.params.control = swiperThumbs
    swiperThumbs.params.control = swiperTop
  }
}

</script>

<style>
#GiftCardSetArea {
  width: 100%;
  float: left;
}

#title_GiftCard,
#GiftCardStyleArea {
  float: left;
  padding: 3.3%;
  width: 93.4%;
}

.sweiperArea_box {
  width: 100%;
  float: left;
  position: relative;
}

.ps_swiperSlide_relative {
  position: relative;
}

.swiper_img_show {
  width: 100px;
  height: 100px;
}

#GiftCardStyleArea {
  background-color: #ffffff;
}

#GiftCardBox {
  position: relative;
  width: 100%;
  float: left;
  padding-top: 100%;
  background: url('../../../static/obj_img/giftcard_img/gift_heka_photo_2.png');
  background-size: contain
}

#GiftCardBox_textArea {
  position: absolute;
  width: 90%;
  padding: 2% 0;
  height: 20%;
  z-index: 2;
  left: 5%;
  background-color: #ffffff;
  opacity: .9;
  display: flex;
  display: -webkit-flex;
  flex-direction: column;
  -webkit-flex-direction: column;
}

#GiftCardBox_textArea textarea {
  flex: 1;
  -webkit-flex: 1;
  font-size: 16px;
  color: #666666;
  padding: 0 10%;
  border: 0;
  outline: none;
  resize: none;
  font-family: "Microsoft YaHei UI Light", sans-serif;
}

#GiftCardBox_textArea input {
  flex: 0.5;
  -webkit-flex: 0.5;
  font-size: 16px;
  color: #666666;
  padding: 0 5%;
  border: 0;
  outline: none
}

#GiftCardBox_textArea input:last-child {
  text-align: right;
}


.swiper-container {
  background-color: #ffffff;
}

.swiper-slide {
  background-size: cover;
  background-position: center;
}

.gallery-top {
  height: 80%!important;
  width: 100%;
}

.gallery-thumbs {
  height: 20%!important;
  box-sizing: border-box;
  padding: 10px 0;
}

.gallery-thumbs .swiper-slide {
  width: 25%;
  height: 100%;
  opacity: 0.4;
}

.gallery-thumbs .swiper-slide-active {
  opacity: 1;
}

#nextStep_GiftCard {
  position: absolute;
  left: 0;
  bottom: 0;
  color: #ffffff;
  background-color: #ff3f5c;
  width: 100%;
  height: 46px;
  line-height: 46px;
  text-align: center;
}
</style>
