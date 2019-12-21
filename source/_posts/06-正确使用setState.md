---
title: 06-正确使用setState
date: 2018-10-19 15:29:38
tags:
  - React.js  
---

关于setState有三件事情需要知道

### 不要直接更新状态
例如，此代码不会重新渲染组件：
``` js
// Wrong
this.state.comment = 'Hello';
```
应当使用 setState():
``` js
// Correct
this.setState({comment: 'Hello'});
```
构造函数是唯一能够初始化 this.state 的地方。

### 状态更新可能是异步的

React 可以将多个setState() 调用合并成一个调用来提高性能。

因为 this.props 和 this.state 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。

例如，此代码可能无法更新计数器：
``` js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

要修复它，请使用第二种形式的 setState() 来接受一个函数而不是一个对象。 该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数：

``` js
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));

```

上方代码使用了箭头函数，但它也适用于常规函数：

``` js
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});

```

### 状态更新合并
当你调用 setState() 时，React 将你提供的对象合并到当前状态。

例如，你的状态可能包含一些独立的变量：
``` js
constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```
你可以调用 setState() 独立地更新它们：

``` js
componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }

```
这里的合并是浅合并，也就是说this.setState({comments})完整保留了this.state.posts，但完全替换了this.state.comments。






