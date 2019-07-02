
# Emotional Sticker(情头贴纸)

## 项目简介
基于微信小程序`canvas api`的头像处理小程序。 可以自由更换头像, 边框和贴纸。

## 运用的技术主要有:
- 采用mpvue + bootstrap技术栈。
- 使用`PostCSS`对CSS进行预处理

## 预览
![](./static/EmotionalSticker.jpg 'EmotionalSticker')

## 运行项目
```
  git clone https://github.com/strawbreey/emotional-sticker.git
  cd emotional-sticker
  npm install
  npm run dev

```
 打开微信开发者工具, 创建新项目。找到当前项目文件中的dist文件

## 上传项目
```
  npm run build
```
 微信开发者中选择`上传`新项目。登录微信公众平台,选择开发管理点击提交审核


## todo

 - 添加用户上传头像
 - 添加输入文字功能
 - 把本地图片素材放到七牛云
 - 使用vuex做数据管理
 - 添加表情包素材
