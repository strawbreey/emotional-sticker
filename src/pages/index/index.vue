<template>
  <div class="container h-auto p-0 bg-light pt-5" style="min-height: 100vh; box-sizing: border-box;">
  
    <canvas class="m-auto rounded shadow" style="width: 300px; height: 300px; overflow: hidden; background-image: url('http://p0jp9nkiy.bkt.clouddn.com/o.png');" canvas-id="firstCanvas" disable-scroll="true"
      @touchstart="touchstart" @touchmove="touchmove" @touchend="touchend" @touchcancel="touchcancel"></canvas>
    
    <div class="fixed-bottom bg-primary box-shadow-top">
      <div class="row text-center m-auto pt-2 text-white" style="width: 440rpx;">
        <div class="col p-0" :class="{'font-weight-bold': active === 'avatar'}" @click="select('avatar')">头像</div>
        <div class="col p-0" :class="{'font-weight-bold': active === 'photo'}" @click="select('photo')">边框</div>
        <div class="col p-0" :class="{'font-weight-bold': active === 'icon'}" @click="select('icon')">贴纸</div>
        <!-- <div class="col p-0" :class="{'font-weight-bold': active === 'text'}" @click="select('text')">文字</div>       -->
      </div>

      <a class="position-absolute reset bg-primary border-light d-flex justify-content-center align-items-center shadow" @click="reset">
        <image src="/static/images/icon/plus.svg" style="width: 80rpx; height: 80rpx;" alt="reset" />        
      </a>
      <a class="position-absolute done bg-primary border-light d-flex justify-content-center align-items-center shadow" @click="next">
        <img src="/static/images/icon/right.svg" style="width: 80rpx; height: 80rpx;" alt="next"/>
      </a>

      
      
      <scroll-view scroll-x class="py-2" style="width: 100vw; height: 224rpx;">
        <div class="d-flex px-2 justify-content-between flex-nowrap px-2">
          <view v-for="(item, index) in list" :item="item" :key="index" class="m-2 p-2 bg-light rounded shadow item">
            <button v-if="item.isAuth" open-type="getUserInfo" @getuserinfo="getuserinfo" style="background-color: transparent;">
              <img class="shadow-sm" style="width: 160rpx; height: 160rpx; overflow: hidden;" :src="item.path" alt="img">
            </button>
            <view v-else @click="select2(index)">
              <img style="width: 160rpx; height: 160rpx; overflow: hidden;" :src="item.path" alt="img">
            </view>
          </view>
        </div>
      </scroll-view>
    </div>
  </div>
</template>

<script>
import store from './store'

export default {
  data () {
    return {
      context: null,
      elements: [],
      avatar: {
        x: 0,
        y: 0,
        type: 'avatar',
        width: 300,
        height: 300,
        path: '/static/images/avatar/1.png',
        trans: 0,
        zIndex: 0
      },
      photo: {
        x: 0,
        y: 0,
        type: 'photo',
        width: 300,
        height: 300,
        path: '/static/images/photo/1.png',
        trans: 0,
        zIndex: 0
      },
      icon: {
        id: 0,
        type: 'icon',
        x: 100,
        y: 100,
        width: 100,
        height: 100,
        path: '',
        trans: 0,
        zIndex: 0,

        scale: 1,
        translateX: 0,
        translateY: 0,
        rotate: 0,
        active: true
      },
      text: {
        type: 'text',
        content: 'content',
        width: '',
        height: '',
        fontsize: '',
        color: '',
        zIndex: ''
      },
      touch: {
        delete: -1,
        method: '',
        element: {
          index: 0,
          width: 0,
          height: 0,
          x: 0,
          y: 0,
          rotate: 0,
          scale: 1
        },
        start: {
          index: 0,
          x: 0,
          y: 0
        },
        move: {
          x: 0,
          y: 0
        }
      },
      avatarList: [
        {
          path: '/static/images/avatar/1.png',
          isAuth: true
        },
        {
          path: '/static/images/avatar/2.jpg'
        },
        {
          path: '/static/images/avatar/3.jpg'
        },
        {
          path: '/static/images/avatar/4.jpg'
        },
        {
          path: '/static/images/avatar/5.jpg'
        },
        {
          path: '/static/images/avatar/6.jpg'
        },
        {
          path: '/static/images/avatar/7.jpg'
        },
        {
          path: '/static/images/avatar/8.jpg'
        },
        {
          path: '/static/images/avatar/9.jpg'
        },
        {
          path: '/static/images/avatar/10.jpg'
        },
        {
          path: '/static/images/avatar/11.jpg'
        }
      ],
      photoList: [
        {
          path: '/static/images/photo/1.png'
        },
        {
          path: '/static/images/photo/2.png'
        },
        {
          path: '/static/images/photo/3.png'
        },
        {
          path: '/static/images/photo/4.png'
        },
        {
          path: '/static/images/photo/5.png'
        },
        {
          path: '/static/images/photo/6.png'
        },
        {
          path: '/static/images/photo/7.png'
        },
        {
          path: '/static/images/photo/8.png'
        },
        {
          path: '/static/images/photo/9.png'
        },
        {
          path: '/static/images/photo/10.png'
        }
      ],
      iconList: [
        {
          path: '/static/images/guajian/1.png'
        },
        {
          path: '/static/images/guajian/2.png'
        },
        {
          path: '/static/images/guajian/3.png'
        },
        {
          path: '/static/images/guajian/4.png'
        },
        {
          path: '/static/images/guajian/5.png'
        },
        {
          path: '/static/images/guajian/6.png'
        },
        {
          path: '/static/images/guajian/7.png'
        },
        {
          path: '/static/images/guajian/8.png'
        },
        {
          path: '/static/images/guajian/9.png'
        },
        {
          path: '/static/images/guajian/10.png'
        },
        {
          path: '/static/images/guajian/11.png'
        },
        {
          path: '/static/images/guajian/12.png'
        },
        {
          path: '/static/images/guajian/13.png'
        },
        {
          path: '/static/images/guajian/14.png'
        },
        {
          path: '/static/images/guajian/15.png'
        },
        {
          path: '/static/images/guajian/16.png'
        },
        {
          path: '/static/images/guajian/17.png'
        },
        {
          path: '/static/images/guajian/18.png'
        },
        {
          path: '/static/images/guajian/19.png'
        },
        {
          path: '/static/images/guajian/20.png'
        },
        {
          path: '/static/images/guajian/21.png'
        },
        {
          path: '/static/images/guajian/22.png'
        },
        {
          path: '/static/images/guajian/23.png'
        },
        {
          path: '/static/images/guajian/24.png'
        },
        {
          path: '/static/images/guajian/25.png'
        }
      ],
      list: null,
      active: 'avatar',
      timer: 0,
      selectId: 0
    }
  },

  computed: {
    list () {
      return store.getters.list
    }
  },

  onShareAppMessage: function (res) {
    return {
      title: '情头贴纸',
      path: '/pages/index/main',
      success: function (res) {
        // 转发成功
      },
      fail: function (res) {
        // 转发失败
      }
    }
  },

  methods: {
    getuserinfo (e) {
      let _this = this
      wx.downloadFile({
        url: e.mp.detail.userInfo.avatarUrl,
        success: function (res) {
          _this.avatarList[0].path = res.tempFilePath
          _this.avatarList[0].isAuth = false
          _this.avatar.path = _this.avatarList[0].path
          _this.drawimage()
        }
      })
    },
    reset () {
      // 初始化
      // this.avatar.path = '/static/images/avatar/1.png'
      // this.photo.path = ''
      // this.elements = []
      // this.drawimage()
      let _this = this
      wx.showActionSheet({
        itemList: ['选择照片', '选择头像'],
        success: function (res) {
          console.log(res.tapIndex)
          if (res.tapIndex === 0) {
            wx.navigateTo({
              url: '/pages/cropper/main'
            })
          } else if (res.tapIndex === 1) {
            _this.getInfo()
          }
        },
        fail: function (res) {
          console.log(res.errMsg)
        }
      })
    },
    drawimage () {
      // 头像
      console.log('drawimage')
      if (this.avatar.path) {
        this.context.drawImage(this.avatar.path, this.avatar.x, this.avatar.y, this.avatar.width, this.avatar.height)
      }
      // 挂件
      for (let item of this.elements) {
        if (item.type === 'icon') {
          this.context.save()
          this.context.translate(item.translateX, item.translateY)
          this.context.rotate(item.rotate * Math.PI / 180)
          this.context.translate(-item.translateX, -item.translateY)

          this.context.drawImage(item.path, item.x, item.y, item.width, item.height)

          // 如果挂件已激活
          if (item.active) {
            this.context.setLineDash([5, 10], 10)
            this.context.setStrokeStyle('#fb5534')
            this.context.setLineCap('round')
            this.context.setLineJoin('round')
            this.context.setLineWidth(2)
            this.context.strokeRect(item.x - 1, item.y - 1, item.width + 2, item.height + 2)
            this.context.drawImage('/static/images/icon/clear.png', item.x - 14, item.y - 14, 28, 28)
            this.context.drawImage('/static/images/icon/drag.png', item.x + item.width - 14, item.y + item.height - 14, 28, 28)
          }
          this.context.restore()
        } else if (item.type === 'text') {
          console.log('text')
        }
      }

      // 相框
      if (this.photo.path) {
        this.context.drawImage(this.photo.path, this.photo.x, this.photo.y, this.photo.width, this.photo.height)
      }

      this.context.draw()
    },
    touchstart (e) {
      // 判断是偏移, 旋转还是放大
      console.log('touchstart')
      // 触摸的开始值
      this.touch.start.x = e.touches[0].x
      this.touch.start.y = e.touches[0].y
      // 初始化触摸的元素
      this.touch.method = ''
      this.touch.element.index = -1
      this.touch.element.width = 0
      this.touch.element.height = 0
      this.touch.element.x = 0
      this.touch.element.y = 0
      this.touch.element.scale = 1
      this.touch.element.rotate = 0

      // for (let [i, item] of new Map(this.elements.map((item, i) => [i, item]))) {
      for (let i = this.elements.length - 1; i !== -1; i--) {
        let item = this.elements[i]
        // 计算物体旋转之后的位置
        let r = Math.sqrt(2) * (item.width / 2)
        let center = {
          x: 0,
          y: 0
        }
        let del = {
          x: 0,
          y: 0
        }

        let move = {
          x: 0,
          y: 0
        }

        center.x = item.x + item.width / 2
        center.y = item.y + item.height / 2

        del.x = center.x + r * Math.sin(Math.PI / 180 * (-45 - item.rotate))
        del.y = center.y - r * Math.cos(Math.PI / 180 * (-45 - item.rotate))

        move.y = center.y + r * Math.sin(Math.PI / 180 * (135 - item.rotate))
        move.x = center.x - r * Math.cos(Math.PI / 180 * (135 - item.rotate))
        if (item && item.active) {
          // 判断删除
          if (e.touches[0].x > (del.x - 16) &&
              e.touches[0].x < (del.x + 16) &&
              e.touches[0].y > (del.y - 16) &&
              e.touches[0].y < (del.y + 16)) {
            this.touch.method = 'clear'
            this.touch.delete = i
            break
          }

          // 判断旋转放大
          if (e.touches[0].x > (move.x - 16) &&
              e.touches[0].x < (move.x + 16) &&
              e.touches[0].y > (move.y - 16) &&
              e.touches[0].y < (move.y + 16)) {
            console.log('旋转放大')

            this.touch.method = 'scale'
            this.touch.element.index = i
            this.touch.element.width = item.width
            this.touch.element.height = item.height

            this.touch.element.x = item.x
            this.touch.element.y = item.y

            this.touch.element.scale = item.scale
            this.touch.element.rotate = item.rotate
            break
          }
        }

        if (e.touches[0].x > center.x - r &&
            e.touches[0].x < center.x + r &&
            e.touches[0].y > center.y - r &&
            e.touches[0].y < center.y + r) {
          console.log('拖动')
          this.touch.method = 'drag'
          this.touch.element.index = i
          this.touch.element.width = item.width
          this.touch.element.height = item.height

          this.touch.element.x = item.x
          this.touch.element.y = item.y

          this.touch.element.scaleX = item.scaleX
          this.touch.element.scaleY = item.scaleY
          this.touch.element.rotate = item.rotate
          break
        }
      }

      // 执行删除命令
      if (this.touch.method === 'clear' && this.touch.delete !== -1) {
        this.elements.splice(this.touch.delete, 1)
        this.touch.delete = -1
      }

      for (let item of this.elements) {
        item.active = false
      }

      if (this.touch.element.index !== -1 && this.touch.element.index < this.elements.length) {
        this.elements[this.touch.element.index].active = true
      }

      console.log('drawimage')
      this.drawimage()
    },
    touchmove (e) {
      if (this.touch.element.index === -1) {
        return
      }
      if (this.touch.method === 'drag') {
        if (e.touches[0].x > 0 && e.touches[0].x < 300 && e.touches[0].y > 0 && e.touches[0].y < 300) {
          this.touch.move.x = e.touches[0].x
          this.touch.move.y = e.touches[0].y

          this.elements[this.touch.element.index].x = this.touch.element.x + this.touch.move.x - this.touch.start.x
          this.elements[this.touch.element.index].y = this.touch.element.y + this.touch.move.y - this.touch.start.y
        }
      } else if (this.touch.method === 'scale') {
        // 旋转放大中
        this.touch.move.x = e.touches[0].x
        this.touch.move.y = e.touches[0].y

        // 放大

        this.elements[this.touch.element.index].x = this.touch.element.x - this.touch.move.x + this.touch.start.x
        this.elements[this.touch.element.index].y = this.touch.element.y - this.touch.move.x + this.touch.start.x

        this.elements[this.touch.element.index].width = (this.touch.move.x - this.touch.start.x) * 2 + this.touch.element.width
        this.elements[this.touch.element.index].height = (this.touch.move.x - this.touch.start.x) * 2 + this.touch.element.height

        // 中心点的坐标
        let centerX = this.touch.element.x + this.touch.element.width / 2
        let centerY = this.touch.element.y + this.touch.element.height / 2

        // 设置原点
        this.elements[this.touch.element.index].translateX = centerX
        this.elements[this.touch.element.index].translateY = centerY

        // 触摸前的角度
        let diffXBefore = this.touch.start.x - centerX
        let diffYBefore = this.touch.start.y - centerY

        let diffXAfter = this.touch.move.x - centerX
        let diffYAfter = this.touch.move.y - centerY

        let angleBefore = Math.atan2(diffYBefore, diffXBefore) / Math.PI * 180
        let angleAfter = Math.atan2(diffYAfter, diffXAfter) / Math.PI * 180

        // this.elements[0].scale = distanceAfter / distanceBefore * this.touch.element.scale
        this.elements[this.touch.element.index].rotate = angleAfter - angleBefore + this.touch.element.rotate
        console.log(this.elements[this.touch.element.index].rotate)
        // 触摸后的角度
      }

      this.drawimage()
    },
    touchend (e) {
      if (this.touch.method === 'drag') {
        console.log('drag')
      } else if (this.touch.method === 'clear') {
        console.log('clear')
      } else if (this.touch.method === 'scale') {
        console.log('scale')
      }
    },
    touchcancel (e) {
      console.log('touchcancel')
    },
    delete () {
      console.log('delete')
      if (this.touch.method === 'clear' && this.touch.delete !== -1) {
        this.elements.splice(this.touch.delete, 1)
        this.touch.delete = -1
      }
      this.drawimage()
    },
    add () {
      console.log('add')
    },
    select (e) {
      console.log(e)
      this.active = e
      if (this.active === 'avatar') {
        this.list = this.avatarList
      } else if (this.active === 'photo') {
        this.list = this.photoList
      } else if (this.active === 'icon') {
        this.list = this.iconList
      }
    },
    select2 (e) {
      console.log(e)
      if (this.active === 'avatar') {
        this.avatar.path = this.avatarList[e].path
      } else if (this.active === 'photo') {
        this.photo.path = this.photoList[e].path
      } else if (this.active === 'icon') {
        let icon = JSON.parse(JSON.stringify(this.icon))
        icon.path = this.iconList[e].path
        icon.id = this.timer
        icon.active = true
        this.timer++
        this.elements.push(icon)
      }
      this.drawimage()
    },
    next () {
      wx.showLoading({
        title: '制作中'
      })
      for (let item of this.elements) {
        item.active = false
      }
      this.drawimage()
      setTimeout(() => {
        wx.canvasToTempFilePath({
          x: 0,
          y: 0,
          width: 300,
          height: 300,
          destWidth: 600,
          destHeight: 600,
          canvasId: 'firstCanvas',
          success: function (res) {
            console.log(res.tempFilePath)
            wx.hideLoading()
            wx.navigateTo({
              url: '/pages/share/main?path=' + res.tempFilePath
            })
          }
        })
      }, 100)
    },
    getInfo () {
      let _this = this
      wx.getSetting({
        success: function (response) {
          if (response.authSetting['scope.userInfo']) {
            wx.getUserInfo({
              success: function (res) {
                console.log(res)
                wx.downloadFile({
                  url: res.userInfo.avatarUrl,
                  success: function (r) {
                    _this.avatarList[0].path = r.tempFilePath
                    _this.avatar.path = _this.avatarList[0].path
                    _this.avatarList[0].isAuth = false
                    _this.drawimage()
                  }
                })
              }
            })
          } else {
            _this.drawimage()
          }
        }
      })
    }
  },

  mounted () {
    this.context = wx.createCanvasContext('firstCanvas')
    if (this.active === 'avatar') {
      this.list = this.avatarList
    } else if (this.active === 'photo') {
      this.list = this.photoList
    } else if (this.active === 'icon') {
      this.list = this.iconList
    }

    if (this.$mp.query.path) {
      console.log(this.$mp.query.path)
      this.avatarList[0].path = this.$mp.query.path
      this.avatar.path = this.avatarList[0].path
      this.drawimage()
    } else {
      this.getInfo()
    }
    // this.drawimage()
  }
}
</script>
<style lang="scss">
.box-shadow-top{
  box-shadow:  0 -0.5rem 2rem rgba(#333, .15);
}

.item{
  background-image: url('http://p0jp9nkiy.bkt.clouddn.com/o.png');
}
.reset{
  top: -75rpx;
  left: 18.5rpx;
  width: 130rpx;
  height: 130rpx;
  border-radius: 50%;
}
.done{
  top: -75rpx;
  right: 18.5rpx;
  width: 130rpx;
  height: 130rpx;
  border-radius: 50%;
}

button{
  padding: 0;
  -webkit-appearance: none;
  border: none;
  &::after{
    content: '';
    border: none;
  }
}
</style>
