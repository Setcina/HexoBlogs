---
title: 03-如何使用Vue.js引用jquery
date: 2018-07-22 19:00:13
tags:
  - Vue.js
---

## 安装以及使用jQuery

### 1.首先安装依赖
``` bash
npm install jquery --save
or
npm install jquery@1.0.9 --save    //@后面是版本号
```

### 2.修改配置
1.修改配置的位置
``` bash
build文件夹下的webpack.base.conf.js文件
加入webpack对象：
var webpack = require("webpack");
```

2.build文件夹下的webpack.base.conf.js文件（原来的位置），在下方module.exports对象里面加入。
``` bash
plugins: [
  // 3. 配置全局使用 jquery
  new webpack.ProvidePlugin({
  $: "jquery",
  jQuery: "jquery",
  jquery: "jquery",
  "window.jQuery": "jquery"
 })],
 //在module.export这个对象下添加
```
3.现在已经可以直接在组件中使用jquery的方法了，不用在其他位置引用jquery，就是这么轻松加愉快。

## 使用jQ插件
关于这一点查阅了很多资料，几乎没什么文献清楚的说明jq插件的使用方式，以至于很多使用vue很久的大佬们，也不知道jq的插件竟然可以直接在vue-cli中使用。。这一步虽然是简单的，但这里还是提一下，为各位提供一些参考。

### 使用方式
jq插件只需要将插件所需要的文件下载到本地src/assets或者最外层的static文件夹中，然后将插件的文件引用进组件，根据插件封装的方法来进行调用就行了，跟直接使用jq的插件基本上是一毛一样的。