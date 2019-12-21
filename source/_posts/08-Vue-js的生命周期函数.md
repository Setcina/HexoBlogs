---
title: 08-Vue.js的生命周期函数
date: 2018-07-25 15:46:18
tags:
 - Vue.js
---


### 生命周期
``` js
钩子函数 生命周期
beforeCreate:function () {
  alert("组件实例化之前执行的函数");
},
created:function(){
  alert("组件实例化完毕，但页面还未显示");
},
beforeMount:function(){
    alert("组件挂载前，页面仍未展示，但虚拟demo已经配置");
},
mounted:function(){
  alert("组件挂载后，此页面展示");
},
beforeUpdate:function () {
  alert("组件更新前，页面仍未更新，但虚拟Dom已经配置");
},
updated:function () {
  alert("组件更新，页面更新");
},
beforeDestory:function(){
    alert("组件销毁前");
},
destoryed:function(){
    alert("组件销毁后");
}

```

[更多生命周期函数](https://cn.vuejs.org/v2/guide/instance.html#%E5%AE%9E%E4%BE%8B%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)
