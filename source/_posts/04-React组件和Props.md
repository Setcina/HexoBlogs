---
title: 04-React组件和Props
date: 2018-10-17 15:27:10
tags:
  - React.js  
---

### 函数定义/类定义组件

定义一个组件最简单的方式是使用js函数

``` js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
该函数是一个有效的React组件，它接收一个单一的“props”对象并返回了一个React元素。我们之所以称这种类型的组件为函数定义组件，是因为从字面上来看，它就是一个JavaScript函数。

<p></p>

你也可以使用 ES6 class 来定义一个组件:

``` js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>
  }
}
```

### 组件渲染

在前面，我们遇到的React元素偶只是DOM标签
``` js
const element = <div />;
```
然而，React元素也可以是用户自定义的组件
``` js
const element = <Welcome name="Sara" />;
```

当React遇到的元素是用户自定义的组件，它会将JSX属性作为单个对象传递给该组件，这个对象称之为“props”。
<p></p>
例如,这段代码会在页面上渲染出”Hello,Sara”

``` js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

我们来回顾一下这个例子中发生了什么
1.我们对<Welcome name="Sara" />元素调用了ReactDOM.render()方法。
2.React将{name: 'Sara'}作为props传入并调用Welcome组件。
3.Welcome组件将< h1 >Hello, Sara</ h1 >元素作为结果返回。
4.React DOM将DOM更新为< h1 >Hello, Sara</ h1 >。

``` bash
组件名称必须以大写字母开头。

例如，<div /> 表示一个DOM标签，但 <Welcome /> 表示一个组件，并且在使用该组件时你必须定义或引入它。

```

### 组合组件
组件可以在它的输出中引用其它组件，这就可以让我们用同一组件来抽象出任意层次的细节。在React应用中，按钮、表单、对话框、整个屏幕的内容等，这些通常都被表示为组件。
<p></p>
例如，我们可以创建一个App组件，用来多次渲染Welcome组件：

``` js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

```
通常，一个新的React应用程序的顶部是一个App组件。但是，如果要将React集成到现有应用程序中，则可以从下而上使用像Button这样的小组件作为开始，并逐渐运用到视图层的顶部。

``` bash
组件的返回值只能有一个根元素。这也是我们要用一个<div>来包裹所有<Welcome />元素的原因。
```

### 提取组件
我们可以将组件分为更小的组件

``` js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
这个组件接收author(对象)、text(字符串)、以及date(Date对象)作为props，用来描述一个社交媒体网站上的评论。
<p></p>
这个组件由于嵌套，变得难以被修改，可复用的部分也难以被复用。所以让我们从这个组件中提取出一些小组件。
<p></p>
首先，我们来提取Avatar组件：

``` js
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />

  );
}
```

Avatar作为Comment的内部组件，不需要知道是否被渲染。因此我们将author改为一个更通用的名字user。
<p></p>
我们建议从组件自身的角度来命名props，而不是根据使用组件的上下文命名。
<p></p>
现在我们可以对Comment组件做一些小小的调整：

``` js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

接下来，我们要提取一个UserInfo组件，用来渲染Avatar旁边的用户名：

``` js
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}

```

这可以让我们进一步简化Comment组件：

``` js
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

提取组件一开始看起来像是一项单调乏味的工作，但是在大型应用中，构建可复用的组件完全是值得的。当你的UI中有一部分重复使用了好几次（比如，Button、Panel、Avatar），或者其自身就足够复杂（比如，App、FeedStory、Comment），类似这些都是抽象成一个可复用组件的绝佳选择，这也是一个比较好的做法。


 


