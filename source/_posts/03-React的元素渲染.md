---
title: 03-React的元素渲染
date: 2018-10-11 14:34:16
tags:
 - React.js
---
### 将元素渲染到 DOM 中
首先我们在一个 HTML 页面中添加一个 id="root" 的:
``` js
<div id="root"></div>
```
在此 div 中的所有内容都将由 React DOM 来管理，所以我们将其称之为 “根” DOM 节点。
<p></p>

我们用React 开发应用时一般只会定义一个根节点。但如果你是在一个已有的项目当中引入 React 的话，你可能会需要在不同的部分单独定义 React 根节点。
<p></p>
要将React元素渲染到根DOM节点中，我们通过把它们都传递给 ReactDOM.render() 的方法来将其渲染到页面上：

``` js
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

### 更新元素渲染
React 元素都是immutable 不可变的。当元素被创建之后，你是无法改变其内容或属性的。一个元素就好像是动画里的一帧，它代表应用界面在某一时间点的样子。

根据我们现阶段了解的有关 React 知识，更新界面的唯一办法是创建一个新的元素，然后将它传入 ReactDOM.render() 方法：

来看一下这个计时器的例子:
``` js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

### React 只会更新必要的部分
React DOM 首先会比较元素内容先后的不同，而在渲染过程中只会更新改变了的部分。
<img src="image01.png" class="myimage">
<p></p>
即便我们每秒都创建了一个描述整个UI树的新元素，React DOM 也只会更新渲染文本节点中发生变化的内容。

根据我们以往的经验，将界面视为一个个特定时刻的固定内容（就像一帧一帧的动画），而不是随时处于变化之中（而不是处于变化中的一整段动画），将会有利于我们理清开发思路，减少各种bug。