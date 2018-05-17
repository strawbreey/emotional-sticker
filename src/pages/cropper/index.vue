<template>
  <div>
    <canvas
        canvas-id="__mpvue_cropper__"
        @touchstart="touchstart"
        @touchmove="touchmove"
        @touchend="touchend"
        disable-scroll
        :style="{ width: cropperOpt.width + 'px', height: cropperOpt.height + 'px', background: 'rgba(0, 0, 0, .8)' }">
    </canvas>
      <div class="cropper-buttons">
        <div
            class="upload"
            @tap="uploadTap">
            上传图片
        </div>
        <div
            class="getCropperImage"
            @tap="getCropperImage">
            生成图片
        </div>
    </div>
  </div>
</template>

<script>
import WeCropper from 'we-cropper'
const CANVAS_ID = '__mpvue_cropper__'

const device = wx.getSystemInfoSync()
const width = device.windowWidth
const height = device.windowHeight - 50
export default {
  name: 'mpvue-cropper',
  data () {
    return {
      _we_cropper: null,
      cropper: null,
      cropperOpt: {
        width,
        height,
        scale: 2.5,
        zoom: 8,
        cut: {
          x: (width - 300) / 2,
          y: (height - 300) / 2,
          width: 300,
          height: 300
        }
      }
    }
  },
  methods: {
    touchstart ($event) {
      this._we_cropper.touchStart($event.mp)
    },
    touchmove ($event) {
      this._we_cropper.touchMove($event.mp)
    },
    touchend ($event) {
      this._we_cropper.touchEnd($event.mp)
    },
    pushOrigin (src) {
      this._we_cropper.pushOrign(src)
    },
    updateCanvas () {
      this._we_cropper.updateCanvas()
    },
    getCropperBase64 () {
      return new Promise((resolve, reject) => {
        this._we_cropper.getCropperImage(src => {
          src ? resolve(src) : reject()
        })
      })
    },
    getCropperImage () {
      this._we_cropper.getCropperImage(src => {
        // src ? resolve(src) : reject()
        console.log(src)
        wx.redirectTo({
          url: '/pages/index/main?path=' + src
        })
      })
    },
    cropperReady (...args) {
      console.log('cropper ready!')
    },
    cropperBeforeImageLoad (...args) {
      console.log('before image load')
    },
    cropperLoad (...args) {
      console.log('image loaded')
    },
    cropperBeforeDraw (...args) {
      // Todo: 绘制水印等等
    },
    uploadTap () {
      let _this = this
      wx.chooseImage({
        count: 1, // 默认9
        sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
        sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
        success: function (res) {
          const src = res.tempFilePaths[0]
          //  获取裁剪图片资源后，给data添加src属性及其值
          _this.pushOrigin(src)
        }
      })
    }
  },
  mounted () {
    this._we_cropper = new WeCropper(Object.assign(this.cropperOpt, {
      id: CANVAS_ID
    }))
      .on('ready', (...args) => {
        this.$emit('ready', ...args)
      })
      .on('beforeImageLoad', (...args) => {
        this.$emit('beforeImageLoad', ...args)
      })
      .on('imageLoad', (...args) => {
        this.$emit('imageLoad', ...args)
      })
      .on('beforeDraw', (...args) => {
        this.$emit('beforeDraw', ...args)
      })
      .updateCanvas()
  }
}
</script>
<style>

.cropper-wrapper{
    position: relative;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    justify-content: center;
    height: 100%;
    background-color: #e5e5e5;
}

.cropper-buttons{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    justify-content: center;
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 50px;
    line-height: 50px;
}

.cropper-buttons .upload, .cropper-buttons .getCropperImage{
    width: 50%;
    text-align: center;
}

.cropper{
    position: absolute;
    top: 0;
    left: 0;
}

.cropper-buttons{
    background-color: #cd3e3a;
    color: #F2F4F5;
}
</style>