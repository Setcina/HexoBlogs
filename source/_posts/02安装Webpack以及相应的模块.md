---
title: 02安装Webpack以及相应的模块
date: 2019-04-09 22:12:33
tags:
    - WebPack
---

#### 安装
``` js
npm install webpack webpack-cli webpack-dev-server
```


* 解释：为什么安装了webpack之后，还要安装webpack-cli以及webpack-dev-server

首先在webpack 3中，webpack本身和它的CLI以前都是在同一个包中，
但在第4版中，为了更好地管理它们，将webpack以及webpack-cli分开

* 第一：
webpack只是构建
webpack-dev-server除了构建，还提供web服务
 
* 第二：webpack.config.json的路径参数
显然，entry都一样，因为都要知道需要构建的文件在哪里
那么区别就在于 output了
 
path和webpack一起，指明构建 之后 输出文件的位置，这是真实的物理地址
 
publickPath和webpack-dev-server一起，当执行webpack-dev-server时，第一步首先跟webpack一样，先构建输出，然后提供web访问，该输出文件是在内存中

默认情况下，不设置publicPath则输出文件默认在运行webpack-dev-server的目录，也就是根目录，那么html中引用直接是src="输出的文件"，，如果设置了publicPath那么html中引用也要相对改变
 
 
总的来说，webpack只是构建，而webpack-dev-server相当于webpack+apache（或者其它web服务器）

区别在于
使用webpack+apache（或者其它服务器），每次构建之后，
* 1 根据path引用构建后的输出文件
* 2 每次修改都要重新运行webpack
 
使用webpack-dev-server，运行之后首先
* 1 先构建，输出文件在内存中，引用构建后的输出文件根据publicPath（默认是根目录）；
* 2 每次修改，自动刷新

