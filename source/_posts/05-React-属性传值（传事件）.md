---
title: 05-React-属性传值（传事件）
date: 2018-09-13 16:23:32
tags:
  - React.js
---

### 如何将事件带的参数带回给事件本身，这里面需要用到bind这个属性，如下图所示

<img class="myimage" src="image01.png">

{ this.switchName.bind(this,"你好")}

### 在方法中接受定义一个形参myName接受这个“你好”的参数,如何在组件中使用属性传值，则需要定义一个属性，并且要定义好事件类型。并且在父组件中直接调用。如下图及上图

<img class="myimage" src="image02.png">