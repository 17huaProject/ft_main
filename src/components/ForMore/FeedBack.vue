<template>
  <div class="feedbackMain">
    <div>在下方输入你宝贵的建议，让一起画变得更好</div>
    <textarea autofocus="autofocus" v-model="feedbackText" maxlength="1000" placeholder="请输入你的反馈，不超过1000字"></textarea>
    <button @click="upLoadText">提交</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      feedbackText: ''
    }
  },
  methods: {
    upLoadText() {
      if (this.feedbackText == '') {
        this.$toast.center('你还未填写任何信息')
        return false
      }
      if (this.feedbackText.length < 15) {
        this.$toast.center('请完整表达')
        return false
      }
      let f = {
        title: '问题反馈',
        content: this.feedbackText,
        access_token: this.$store.state.usrInfomation.access_token
      }
      this.$store.commit('ConectionPara', f)
      let getString = this.$store.state.getParamString;
      let postString = getString.replace('?', '');
      this.$http.post(this.$store.state.serverHost + '/yqhbsp/custom/feedback', postString).then(m => this.reCallBack(m.data)).catch(r => console.log(r));
    },
    reCallBack(data) {
      if (data.errcode === 200) {
        this.$toast.bottom('感谢您的反馈，谢谢')
        this.$router.go(-1)
      } else {
        this.$toast.center(data.errmsg)
      }

    }
  }
}
</script>

<style>
.feedbackMain {
  padding: 0 3.3%;
  width: 93.4%;
  background-color: #ffffff;
  outline: none;
  min-height: 100vh
}

.feedbackMain div {
  padding-top: 3.3%;
  width: 100%;
  background-color: #ffffff;
}

.feedbackMain textarea {
  padding: 3.3%;
  margin: 3.3% 0 0 0;
  width: 93.4%;
  height: 300px;
  background-color: #f2f2f2;
  border: 1px solid #eeeeee;
  font-size: 16px;
  color: #666666;
  resize: none;
  outline: none;
  font-family: "Microsoft YaHei UI Light", sans-serif;
}

.feedbackMain button {
  width: 100%;
  border: 0;
  line-height: 40px;
  margin-top: 3.3%;
  box-shadow: 0 0 0 1px #ff3f5c;
  background-color: #ff3f5c;
  color: #ffffff;
  font-size: 16px;
  outline: none
}
</style>
