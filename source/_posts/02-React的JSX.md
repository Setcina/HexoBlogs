---
title: 02-React的JSX
date: 2018-10-11 10:36:21
tags:
  - React.js
---
### JSX简介
我们来观察一下声明的这个变量：

``` js
const element = <h1>Hello, world!</h1>
```

这种看起来可能有些奇怪的标签语法既不是字符串也不是 HTML。
<p></p>
它被称为 JSX， 一种 JavaScript 的语法扩展。 我们推荐在 React 中使用 JSX 来描述用户界面。JSX 乍看起来可能比较像是模版语言，但事实上它完全是在 JavaScript 内部实现的。
<p></p>
JSX 用来声明 React 当中的元素。我们先来看看 JSX 的基本使用方法。

### 在 JSX 中使用表达式
你可以任意地在 JSX 当中使用 JavaScript 表达式，在 JSX 当中的表达式要包含在大括号里。

``` js
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>Hello, {formatName(user)}!</h1>
  <h1>{user.firstName}</h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);

``` 

### JSX 本身其实也是一种表达式
在编译之后呢，JSX 其实会被转化为普通的 JavaScript 对象。

这也就意味着，你其实可以在 if 或者 for 语句里使用 JSX，将它赋值给变量，当作参数传入，作为返回值都可以：
``` js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### JSX 属性
你可以使用引号来定义以字符串为值的属性：

``` js
const element = <div tabIndex="0"></div>;
```
也可以使用大括号来定义以 JavaScript 表达式为值的属性：

``` js
const element = <img src={user.avatarUrl}></img>;
```

切记你使用了大括号包裹的 JavaScript 表达式时就不要再到外面套引号了。JSX 会将引号当中的内容识别为字符串而不是表达式。

### JSX 嵌套
如果 JSX 标签是闭合式的，那么你需要在结尾处用 />, 就好像 XML/HTML 一样：

``` js
const element = <img src={user.avatarUrl} />;
```

JSX 标签同样可以相互嵌套：

``` js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

警告:

因为 JSX 的特性更接近 JavaScript 而不是 HTML , 所以 React DOM 使用 camelCase 小驼峰命名 来定义属性的名称，而不是使用 HTML 的属性名称。

例如，class 变成了 className，而 tabindex 则对应着 tabIndex。

