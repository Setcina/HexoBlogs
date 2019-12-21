---
title: 04-vue-router到底是什么
date: 2018-07-23 20:34:31
tags:
  - Vue.js
---

### 介绍
Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：
1.嵌套的路由/视图表
2.模块化的、基于组件的路由配置
3.路由参数、查询、通配符
4.基于 Vue.js 过渡系统的视图过渡效果
5.细粒度的导航控制
6.带有自动激活的 CSS class 的链接
7.HTML5 历史模式或 hash 模式，在 IE9 中自动降级
8.自定义的滚动条行为

### HTML5 History 模式
vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 history 模式，这种模式充分利用 history.pushState API 来完成 URL 跳转而无须重新加载页面。
```
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```
当你使用 history 模式时，URL 就像正常的 url，例如 http://yoursite.com/user/id，也好看！

不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 http://oursite.com/user/id 就会返回 404，这就不好看了。

所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。

### 题外话，hash以及history模式

随着 ajax 的使用越来越广泛，前端的页面逻辑开始变得越来越复杂，特别是spa的兴起，前端路由系统随之开始流行。

从用户的角度看，前端路由主要实现了两个功能（使用ajax更新页面状态的情况下）：

1.记录当前页面的状态（保存或分享当前页的url，再次打开该url时，网页还是保存（分享）时的状态）；
2.可以使用浏览器的前进后退功能（如点击后退按钮，可以使页面回到使用ajax更新页面之前的状态，url也回到之前的状态）；
作为开发者，要实现这两个功能，我们需要做到：

1.改变url且不让浏览器向服务器发出请求；
2.监测 url 的变化；
3.截获 url 地址，并解析出需要的信息来匹配路由规则。
我们路由常用的hash模式和history模式实际上就是实现了上面的功能。

### hash模式
这里的 hash 就是指 url 尾巴后的 # 号以及后面的字符。这里的 # 和 css 里的 # 是一个意思。hash 也 称作 锚点，本身是用来做页面定位的，她可以使对应 id 的元素显示在可视区域内。

由于 hash 值变化不会导致浏览器向服务器发出请求，而且 hash 改变会触发 hashchange 事件，浏览器的进后退也能对其进行控制，所以人们在 html5 的 history 出现前，基本都是使用 hash 来实现前端路由的。

使用到的api：

``` bash
window.location.hash = 'qq' // 设置 url 的 hash，会在当前url后加上 '#qq'

var hash = window.location.hash // '#qq'  

window.addEventListener('hashchange', function(){ 
    // 监听hash变化，点击浏览器的前进后退会触发
})
```

### history
已经有 hash 模式了，而且 hash 能兼容到IE8， history 只能兼容到 IE10，为什么还要搞个 history 呢？
首先，hash 本来是拿来做页面定位的，如果拿来做路由的话，原来的锚点功能就不能用了。其次，hash 的传参是基于 url 的，如果要传递复杂的数据，会有体积的限制，而 history 模式不仅可以在url里放参数，还可以将数据存放在一个特定的对象中。
最重要的一点：

如果不想要很丑的 hash，我们可以用路由的 history 模式
—— 引用自 vueRouter文档

相关API：
``` bash
window.history.pushState(state, title, url) 
// state：需要保存的数据，这个数据在触发popstate事件时，可以在event.state里获取
// title：标题，基本没用，一般传 null
// url：设定新的历史记录的 url。新的 url 与当前 url 的 origin 必须是一樣的，否则会抛出错误。url可以是绝对路径，也可以是相对路径。
//如 当前url是 https://www.baidu.com/a/,执行history.pushState(null, null, './qq/')，则变成 https://www.baidu.com/a/qq/，
//执行history.pushState(null, null, '/qq/')，则变成 https://www.baidu.com/qq/

window.history.replaceState(state, title, url)
// 与 pushState 基本相同，但她是修改当前历史记录，而 pushState 是创建新的历史记录

window.addEventListener("popstate", function() {
    // 监听浏览器前进后退事件，pushState 与 replaceState 方法不会触发              
});

window.history.back() // 后退
window.history.forward() // 前进
window.history.go(1) // 前进一步，-2为后退两步，window.history.lengthk可以查看当前历史堆栈中页面的数量
```
history 模式改变 url 的方式会导致浏览器向服务器发送请求，这不是我们想看到的，我们需要在服务器端做处理：如果匹配不到任何静态资源，则应该始终返回同一个 html 页面。

