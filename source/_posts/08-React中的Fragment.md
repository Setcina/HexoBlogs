---
title: 09-React中的Fragment
date: 2018-09-28 20:20:58
tags:
   - React.js
---

### Fragments的作用

React 中一个常见模式是为一个组件返回多个元素。Fragments 可以让你聚合一个子元素列表，并且不在DOM中增加额外节点。

使用如下：
``` js
render() {                      
  return (                      
    <>                          
      <ChildA />                
      <ChildB />                
      <ChildC />                
    </>                         
  );                            
}   
```
Fragments 看起来像空的 JSX 标签，但是我们在一个组件中返回子元素列表的时候，为了有效的渲染，会有一个父级的容器存在，那么我们经常会这样写

``` js
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Hello</td>
        <td>World</td>
      </div>
    );
  }
}
```

这种时候，输出的table里面，会存在div这个容器，这显然是有问题的。

为了在一个table里面正确的输出子元素，这个时候就要借助Fragment，使用如下：
``` js
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

但是，这样的使用，会存在一个问题是，这样的语法没办法接受键值和属性。
如果，我们需要一个带key的片段，那么可以用这样使用

``` js
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment key={item.id}>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
```

这样子，我们就可以在模板中引用了
``` js
class Tables extends React.Component {
  render() {
    return (
      <table>
        <Columns/>
      </table>
    );
  }
}
```