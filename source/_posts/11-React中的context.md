---
title: 11-React中的context
date: 2018-12-06 14:02:49
tags:
  - React.js
---

### 前言
Context被翻译为上下文，在编程领域，这是一个经常会接触到的概念，React中也有。

在React的官方文档中，Context被归类为高级部分(Advanced)，属于React的高级API，但官方并不建议在稳定版的App中使用Context。

> The vast majority of applications do not need to use content.

If you want your application to be stable, don't use context. It is an experimental API and it is likely to break in future releases of React.

不过，这并非意味着我们不需要关注Context。事实上，很多优秀的React组件都通过Context来完成自己的功能，比如react-redux的<Provider />，就是通过Context提供一个全局态的store，拖拽组件react-dnd，通过Context在组件中分发DOM的Drag和Drop事件，路由组件react-router通过Context管理路由状态等等。在React组件开发中，如果用好Context，可以让你的组件变得强大，而且灵活。

### React Context

官方对于Context的定义

React文档官网并未对Context给出“是什么”的定义，更多是描述使用的Context的场景，以及如何使用Context。

官网对于使用Context的场景是这样描述的：
 > In Some Cases, you want to pass data through the component tree without having to pass the props down manuallys at every level. you can do this directly in React with the powerful "context" API.

简单说就是，当你不想在组件树中通过逐层传递props或者state的方式来传递数据时，可以使用Context来实现跨层级的组件数据传递。

<img data-original-src="//upload-images.jianshu.io/upload_images/1457831-b19e007758f57df7" data-original-width="1930" data-original-height="1000" data-original-format="image/jpeg" data-original-filesize="125723" class="" style="cursor: zoom-in;" src="//upload-images.jianshu.io/upload_images/1457831-b19e007758f57df7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp">

使用props或者state传递数据，数据自顶下流。

<img data-original-src="//upload-images.jianshu.io/upload_images/1457831-1b8e3c5ce88f7758" data-original-width="1626" data-original-height="1000" data-original-format="image/jpeg" data-original-filesize="95522" class="" style="cursor: zoom-in;" src="//upload-images.jianshu.io/upload_images/1457831-1b8e3c5ce88f7758?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp">

使用Context，可以跨越组件进行数据传递。

### 如何使用Context
如果要Context发挥作用，需要用到两种组件，一个是Context生产者(Provider)，通常是一个父节点，另外是一个Context的消费者(Consumer)，通常是一个或者多个子节点。所以Context的使用基于生产者消费者模式。

对于父组件，也就是Context生产者，需要通过一个静态属性childContextTypes声明提供给子组件的Context对象的属性，并实现一个实例getChildContext方法，返回一个代表Context的纯对象 (plain object) 。

``` js
import React from 'react'
import PropTypes from 'prop-types'

class MiddleComponent extends React.Component {
  render () {
    return <ChildComponent />
  }
}

class ParentComponent extends React.Component {
  // 声明Context对象属性
  static childContextTypes = {
    propA: PropTypes.string,
    methodA: PropTypes.func
  }
  
  // 返回Context对象，方法名是约定好的
  getChildContext () {
    return {
      propA: 'propA',
      methodA: () => 'methodA'
    }
  }
  
  render () {
    return <MiddleComponent />
  }
}


```
而对于Context的消费者，通过如下方式访问父组件提供的Context。

``` js
import React from 'react'
import PropTypes from 'prop-types'

class ChildComponent extends React.Component {
  // 声明需要使用的Context属性
  static contextTypes = {
    propA: PropTypes.string
  }
  
  render () {
    const {
      propA,
      methodA
    } = this.context
    
    console.log(`context.propA = ${propA}`)  // context.propA = propA
    console.log(`context.methodA = ${methodA}`)  // context.methodA = undefined
    
    return ...
  }
}

```