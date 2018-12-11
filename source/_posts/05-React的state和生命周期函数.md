---
title: 06-React的state和生命周期函数
date: 2018-10-17 16:50:52
tags:
  - React.js  
---

到目前为止我们只学习了一种方法来更新UI。

我们调用 ReactDOM.render() 方法来改变输出：

``` js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

在本节中，我们将学习如何使Clock组件真正可重用和封装。它将设置自己的计时器，并每秒钟更新一次。

我们可以从封装时钟开始：

``` js
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
然而，它错过了一个关键的要求：Clock设置一个定时器并且每秒更新UI应该是Clock的实现细节。

理想情况下，我们写一次 Clock 然后它能更新自身：

``` js
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

为了实现这个需求，我们需要为Clock组件添加状态

状态与属性十分相似，但是状态是私有的，完全受控于当前组件。

我们之前提到过，定义为类的组件有一些特性。局部状态就是如此：一个功能只适用于类。

### 将函数转换为类

你可以通过5个步骤将函数组件 Clock 转换为类

1.创建一个名称扩展为 React.Component 的ES6 类

2.创建一个叫做render()的空方法

3.将函数体移动到 render() 方法中

4.在 render() 方法中，使用 this.props 替换 props

5.删除剩余的空函数声明
``` js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Clock 现在被定义为一个类而不只是一个函数

使用类就允许我们使用其它特性，例如局部状态、生命周期钩子

### 为一个类添加局部状态

我们会通过3个步骤将 date 从属性移动到状态中：
1.在 render() 方法中使用 this.state.date 替代 this.props.date
``` js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
2.添加一个类构造函数来初始化状态 this.state
``` js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

```

注意我们如何传递 props 到基础构造函数的：

``` js
constructor(props) {
    super(props);
    this.state = {date: new Date()};
}
```
类组件应始终使用props调用基础构造函数。

3.从 <Clock /> 元素移除 date 属性：
``` js
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
稍后将定时器代码添加回组件本身。

``` js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

接下来，我们将使Clock设置自己的计时器并每秒更新一次。

### 将生命周期方法添加到类中
在具有许多组件的应用程序中，在销毁时释放组件所占用的资源非常重要。

每当Clock组件第一次加载到DOM中的时候，我们都想生成定时器，这在React中被称为挂载

同样，每当Clock生成的这个DOM被移除的时候，我们也会想要清除定时器，这在React中被称为卸载。

我们可以在组件类上声明特殊的方法，当组件挂载或卸载时，来运行一些代码：

``` js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

这些方法被称作生命周期钩子。

当组件输出到 DOM 后会执行 componentDidMount() 钩子，这是一个建立定时器的好地方：

``` js
componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

注意我们如何在 this 中保存定时器ID。

虽然 this.props 由React本身设置以及this.state 具有特殊的含义，但如果需要存储不用于视觉输出的东西，则可以手动向类中添加其他字段。

如果你不在 render() 中使用某些东西，它就不应该在状态中。

我们将在 componentWillUnmount()生命周期钩子中卸载计时器：

``` js
componentWillUnmount() {
    clearInterval(this.timerID);
}
```

最后，我们实现了每秒钟执行的 tick() 方法。

它将使用 this.setState() 来更新组件局部状态：

``` js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

现在时钟每秒钟都会执行。

让我们快速回顾一下发生了什么以及调用方法的顺序：

1.当 <Clock /> 被传递给 ReactDOM.render() 时，React 调用 Clock 组件的构造函数。 由于 Clock 需要显示当前时间，所以使用包含当前时间的对象来初始化 this.state 。 我们稍后会更新此状态。

2.React 然后调用 Clock 组件的 render() 方法。这是 React 了解屏幕上应该显示什么内容，然后 React 更新 DOM 以匹配 Clock 的渲染输出。

3.当 Clock 的输出插入到 DOM 中时，React 调用 componentDidMount() 生命周期钩子。 在其中，Clock 组件要求浏览器设置一个定时器，每秒钟调用一次 tick()。

4.浏览器每秒钟调用 tick() 方法。 在其中，Clock 组件通过使用包含当前时间的对象调用 setState() 来调度UI更新。 通过调用 setState() ，React 知道状态已经改变，并再次调用 render() 方法来确定屏幕上应当显示什么。 这一次，render() 方法中的 this.state.date 将不同，所以渲染输出将包含更新的时间，并相应地更新DOM。

5.一旦Clock组件被从DOM中移除，React会调用componentWillUnmount()这个钩子函数，定时器也就会被清除。