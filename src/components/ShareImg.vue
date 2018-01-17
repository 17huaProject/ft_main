<template>
  <div id="ShareImgArea">
     <!-- <img class="ShareTitleContent" src="//image.17hua.me/upload/image/201711/15111445854341017.png"> -->
      <div class="ShareWaitTime">
          正在载入图片 <i class="el-icon-loading"></i>
      </div>
     <img class="ShareImgContent" :src="imgAddr.replace('http:','')">
     <div class="ShareRuleText">
       <div>01、<span>长按保存</span>上方的专属二维码<span>并转发</span>至你的好友或朋友圈</div>
       <div>02、好友通过你发出的海报扫码关注即可获得<span>5元代金券</span>可以在商城任意一次下单中使用</div>
     </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imgAddr: ""
    };
  },
  created() {
    let a = {
      access_token: this.$store.state.usrInfomation.access_token
    };
    this.$store.commit("ConectionPara", a);
    let getString = this.$store.state.getParamString;
    let postString = getString.replace("?", "");
    this.$http
      .get(
        this.$store.state.serverHost +
          "/yqhbsp/userinfo/createShare" +
          getString
      )
      .then(m => this.setImgInThisArea(m.data))
      .catch(r => console.log(r));
  },
  methods: {
    setImgInThisArea(data) {
      if (data.errcode == 200) {
        this.imgAddr = data.record.share_img;
        // this.convertImgToBase64(data.data, function(base64Img) {

        // });
      } else {
        this.$toast.center(data.message);
      }
    },
    convertImgToBase64(url, callback, outputFormat) {
      var canvas = document.createElement("CANVAS"),
        ctx = canvas.getContext("2d"),
        img = new Image();
      img.crossOrigin = "Anonymous";
      img.onload = function() {
        canvas.height = img.height;
        canvas.width = img.width;
        ctx.drawImage(img, 0, 0);
        var dataURL = canvas.toDataURL(outputFormat || "image/png");
        callback.call(this, dataURL);
        canvas = null;
      };
      img.src = url;
    }
  }
};
</script>

<style>
#ShareImgArea {
  width: 100%;
  background: url("//image.17hua.me/upload/image/201711/1511144585522683.png");
  background-size: 100% 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
}
.ShareWaitTime {
  color: #ff3f5c;
  position: absolute;
  width: 100%;
  top: 50%;
  left: 0;
  text-align: center;
  height: 40px;
  line-height: 40px;
  margin-top: -20px;
  z-index: -1;
}
.ShareTitleContent {
  width: 70%;
  padding: 30px 0;
}
.ShareImgContent {
  width: 76%;
}
.ShareRuleText {
  width: 76%;
  color: #ffffff;
  margin-bottom: 20px;
  font-size: 14px;
  text-align: justify;
  line-height: 18px;
  letter-spacing: 2.25px;
  font-weight: bold;
}
.ShareRuleText div{
  margin-top: 20px;
}
.ShareRuleText span{
  color: #ff3f5c
}
</style>
