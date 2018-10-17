---
title: 05-React-属性传值（传事件）
date: 2018-09-13 16:23:32
tags:
  - React.js
---

### 如何将事件带的参数传回给事件本身，如下图所示

<img class="myimage" src="image01.png">
<p> </p>
我们先解析一下上面的代码
我们可以看到<User myClick = {this.switchName.bind(this,"Exiaer") }/>绑定了一个属性myClick，而这个属性值所绑定的其实是一个事件switchName
``` js
{ this.switchName.bind(this,"Exiaer")}
```

### 如何使用

<img class="myimage" src="image02.png">
<p> </p>
解析上面代码
在方法中接受定义一个形参myClick接受这个“Exiaer”的参数
我们可以直接在这种无状态组件中，直接使用{props.myClick}获取到父组件传过来的值，当然了，如果要进行修改的话，其实都是需要通过父组件的方法，所以我们要了解到一件事情，当我们属性值修改的时候
我们一般是会调用到父组件的事件来进行数据的更改

