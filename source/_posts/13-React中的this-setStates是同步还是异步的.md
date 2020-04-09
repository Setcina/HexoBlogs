---
title: 13-React中的this.setStates是同步还是异步的
date: 2020-04-08 14:35:15
tags:
 - React.js
---

#### 执行setState()的位置?
* 在react控制的回调函数中: 生命周期勾子 / react事件监听回调
* 非react控制的异步回调函数中: 定时器回调 / 原生DOM事件监听回调 / promise回调 /...

#### 异步 OR 同步?
* react相关回调中（生命周期回调、事件监听回调）: 异步
* 其它异步回调中（定时器、原生DOM事件监听回调、Promsie回调）: 同步

#### 验证
``` js
import React, { Component } from 'react'

class Demo extends Component {
    state = {
        count:0
    }
    /**异步更新：react事件监听回调里，setState是异步更新的 */
    update1 = () => {
        console.log('update1 更新前',this.state.count)
        this.setState(state=>({count:state.count+1}))
        console.log('update1 更新后',this.state.count)
    }
    /**异步更新：react生命周期回调函数里，setState是异步更新的 */
    componentDidMount(){
        console.log('componentDidMount 更新前',this.state.count)
        this.setState(state=>({count:state.count+1}))
        console.log('componentDidMount 更新后',this.state.count)
    }
    /**同步更新：定时器回调  */
    update2 = () => {
        setTimeout(()=>{
            console.log('定时器 更新前',this.state.count)
            this.setState(state=>({count:state.count+1}))
            /**setState导致状态更新流程触发，更新完毕后才执行下面代码，所以这里为同步更新 */
            console.log('定时器 更新后',this.state.count)
        },3000)
    }
    /**同步更新：原生DOM事件监听回调---结合ref  */
    update3 = () => {
        let count_dom = this.refs.count_dom
        count_dom.onclick = () => {
            console.log('原生DOM事件监听回调 更新前',this.state.count)
            this.setState(state=>({count:state.count+1}))
            console.log('原生DOM事件监听回调 更新后',this.state.count)
        }
    }
    /**同步更新：promise回调  */
    update4 = () => {
        Promise.resolve().then(value=>{
            console.log('promise 更新前',this.state.count)
            this.setState(state=>({count:state.count+1}))
            console.log('promise 更新后',this.state.count)
        })
    }
    render() {
        const {count} = this.state
        console.log('render渲染',count)
        return ( 
            <>
                <h2 ref="count_dom">{count}</h2>
                <button onClick={this.update1}>更新1</button>
                <button onClick={this.update2}>更新2</button>
                <button onClick={this.update3}>更新3</button>
                <button onClick={this.update4}>更新4</button>
                <button onClick={this.update5}>更新5</button>
            </>
        );
    }
}
 
export default Demo;
```

我们也可以看一道面试题，看看this.setState的顺序[面试题](https://www.cnblogs.com/jianxian/p/12630439.html)