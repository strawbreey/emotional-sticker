<template>
  <div class="container h-auto p-0 bg-light pt-5" style="min-height: 100vh; box-sizing: border-box;">
    <div class="text-center">
      <img :src="path" alt="result" @click="preview" class="bg-white shadow" style="height: 300px; width: 300px"/>
    </div>
    <p class="pt-4 text-center text-secondary">提示: 预览图片长按即可保存</p>
    <div class="fixed-bottom bg-primary text-center d-flex justify-content-center align-items-center" style="height: 100px">
      <a class="back position-absolute bg-primary d-flex justify-content-center align-items-center shadow" @click="back">
        <img src="/static/images/icon/back.svg" style="width: 80rpx; height: 80rpx; transform:rotate(90deg);" alt="next"/>        
      </a>
      <button class="share position-absolute bg-primary d-flex justify-content-center align-items-center shadow"  open-type="share">
        <img src="/static/images/icon/share.svg" style="width: 80rpx; height: 80rpx;" alt="next"/>
      </button>
      <button class="btn text-light btn-link font-weight-light" send-message-title="title" open-type="contact">联系客服</button> 
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      path: ''
    }
  },

  onShareAppMessage: function (res) {
    return {
      title: '情头贴纸',
      path: '/pages/index/main',
      imageUrl: this.path,
      success: function (res) {
        // 转发成功
      },
      fail: function (res) {
        // 转发失败
      }
    }
  },

  methods: {
    back () {
      wx.navigateBack({
        delta: 1
      })
    },
    share () {
      console.log('share')
    },
    preview () {
      console.log('preview')
      wx.previewImage({
        current: this.path, // 当前显示图片的http链接
        urls: [this.path] // 需要预览的图片http链接列表
      })
    }
  },

  mounted () {
    this.path = this.$mp.query.path
  },

  onHide () {
    // this.path = ''
  }
}
</script>
<style lang="scss">
.back{
  top: -75rpx;
  left: 75rpx;
  width: 130rpx;
  height: 130rpx;
  border-radius: 50%;
}
.share{
  top: -75rpx;
  right: 75rpx;
  width: 130rpx;
  height: 130rpx;
  border-radius: 50%;
}
button{
  -webkit-appearance: none;
  border: none;
  // color: white;
  &::after{
    content: '';
    border: none;
  }
}
</style>
