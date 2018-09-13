---
title: 04-React-state状态的使用
date: 2018-09-13 11:19:15
tags:
 - React.js
---

首先我们要明白state和props之间很相似，容易混淆。
属性更多的是用来组件之间的通信
state并不是用来传值的，而是改变一个值的状态

### 首先定义state

<img class="myimage" src="image01.png">

### 如何在取值，使用{this.state.Users[0].name}可以取到值

<img class="myimage" src="image02.png">

### 如果要更改state里面的值，我们可以调用一个方法，并且使用this.setState修改里面的值

<img class="myimage" src="image03.png">