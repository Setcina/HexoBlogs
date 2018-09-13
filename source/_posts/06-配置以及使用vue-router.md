---
title: 06-配置以及使用vue-router
date: 2018-07-24 08:25:03
tags:
  - Vue.js
---

### 1.如何使用vue-router

``` import
首先安装vue-router模块（npm install vue-router --save-dev）
import VueRouter from 'vue-router'
Vue.use(VueRouter)
const router = new VueRouter({
  routes:Routers,
  mode:"history"
})
```

### 2.定义常量router建立理由js routers.js

router.js由模板以及数组对象组成

``` routers.js
import ShowBlogs from './components/ShowBlogs'
import AddBlogs from './components/AddBlogs'
import SingleBlog from './components/SingleBlog'
export default [
  {path:'/',component:ShowBlogs},
  {path:'/add',component:AddBlogs},
  {path:'/blog/:id',component:SingleBlog}
]
```

### 3.在页面使用
``` html
<router-link to='/'></router-link>
作为路由链接

<router-view></router-view>
作为路由出口
```
