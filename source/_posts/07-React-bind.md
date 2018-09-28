---
title: 07-React.bind()
date: 2018-09-27 21:44:35
tags:
   - React.js
---

### 1
当使用es5时，是不需要用.bind()的。
当使用es5 时，React会自动帮助我们给每一个function绑定一个this，所以我们不再需要手动绑定。

``` js
var HelloWorld = React.createClass({
  getInitialState() {
    return { message: 'Hi' };
  },

  logMessage() {
    // this magically works because React.createClass autobinds.
    console.log(this.state.message);
  },

  render() {
    return (
      <input type="button" value="Log" onClick={this.logMessage} />
    );
  }
});

```


### 2
当使用es6时，es6不会再自动的帮助我们绑定函数，这时我们就需要使用.bind(this).
``` js
class HelloWorld extends React.Component {
  constructor(props) {
    super(props);
    this.state = { message: 'Hi' };
  }

  logMessage() {
    // This works because of the bind in render below.
    console.log(this.state.message);
  }

  render() {
    return (
      <input type="button" value="Log" onClick={this.logMessage.bind(this)} />
    );
  }
}

```

### 3
在es6中，我们也可以使用箭头函数来避免使用.bind(this)

``` js
class HelloWorld extends React.Component {
  constructor(props) {
    super(props);
    this.state = { message: 'Hi' };
  }

  logMessage() {
    // This works because of the arrow function in render below.
    console.log(this.state.message);
  }

  render() {
    return (
      <input type="button" value="Log" onClick={() => this.logMessage()} />
    );
  }
}

```

同样,我们也可以在logMessage的实现中使用箭头函数。

``` js
class HelloWorld extends React.Component {
  // Note that state is a property,
  // so no constructor is needed in this case.
  state = {
    message: 'Hi'
  };

  logMessage = () => {
    // This works because arrow funcs adopt the this binding of the enclosing scope.
    console.log(this.state.message);
  };

  render() {
    return (
      <input type="button" value="Log" onClick={this.logMessage} />
    );
  }
}
```
本文来着简书
[原文链接](https://www.jianshu.com/p/3a7e97f3995d/)
