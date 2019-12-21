---
title: 03-React组件通信
date: 2018-09-13 09:51:17
tags:
 - React.js
---

组件之间的通信可以通过属性进行传值
### 1.首先在调用组件的标签里，写入属性，如下图所示
<img class="myimage" src="image01.png">

### 最后在组件里面使用 { props.name } 拿到该属性的值，如下图所示
<img class="myimage" src="image02.png">