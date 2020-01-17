---
title: 01-React Hooks 是什么
date: 2019-11-24 22:20:47
tags:
  React Hooks
---

### Hook简介
Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。 e.g

``` js
import React, { useState } from 'react';

function Example() {
  // 声明一个新的叫做 “count” 的 state 变量
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
示意：在这里useState是一个Hook，React将在重新渲染之间保留这个useState。而useState返回的是当前状态值（count）以及用来更新的（setCount）。你可以从事件处理程序或其他位置调用此函数。这个函数类似于类中的this.setState，但是它不会将旧状态和新状态合并在一起。

#### 声明多个state
``` js
function ExampleWithManyStates() {
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

#### 什么是Hook
首先我们知道，在没有Hook之前，我们在书写react中，经常会用到class component 以及 function component。而在
function component，我们多数情况都是从父级中把props传入，进行一些页面渲染。我们无法对props进行状态维护以及更新，因为这些操作都是依赖着父级组件的state。那么有了Hook之后，我们可以在function component挂钩React状态和生命周期功能的功能。钩子在类内部不起作用 - 它们允许你在没有类的情况下使用React。

#### useEffect
订阅或手动更改DOM。我们将这些操作称为“副作用”（或简称为“效果”），因为它们会影响其他组件，并且在渲染过程中无法完成。它与React类中的componentDidMount，componentDidUpdate和componentWillUnmount 具有相同的用途，但是统一成一个Api中。

``` js
import React, { useState，useEffect } from 'react';

function Example() {
  // 声明一个新的叫做 “count” 的 state 变量
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count])

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

在组件渲染之后，将会执行useEffect，将我们的文档title修改成为"你点击了多少次"，那么我们应该也可以注意到，在useEffect后面，有一个[count]，这个其实是说，在count变更的时候，将会执行useEffect里面的js。这就相当于说，我们告诉了useEffect，我们什么时候需要刷新组件。


