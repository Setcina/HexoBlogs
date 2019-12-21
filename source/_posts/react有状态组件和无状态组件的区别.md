---
title: 12-react有状态组件和无状态组件的区别
date: 2019-03-07 22:35:47
tags:
    - React.js
---
React中的组件主要分为无状态组件和有状态组件两类。

无状态组件：无状态组件(Stateless Component)是最基础的组件形式，由于没有状态的影响所以就是纯静态展示的作用。一般来说，各种UI库里也是最开始会开发的组件类别。如按钮、标签、输入框等。它的基本组成结构就是属性（props）加上一个回调函数（return）。由于不涉及到状态的更新，所以这种组件的复用性也最强。

``` js
export const Header=(props)=>{  
    return( 
        <div>无状态组件</div>
    )
}
```

有状态组件：在无状态组件的基础上，如果组件内部包含状态（state）且状态随着事件或者外部的消息而发生改变的时候，这就构成了有状态组件（Stateful Component）。有状态组件通常会带有生命周期(lifecycle)，用以在不同的时刻触发状态的更新。这种组件也是通常在写业务逻辑中最经常使用到的，根据不同的业务场景组件的状态数量以及生命周期机制也不尽相同。
``` js
export class Home extends React.Component {
    constructor(props) {
        super(props);
    };
    render() {
        return (
            <Header/>  //也可以写成<Header></Header>
        )
    }
}
```

那么其实在react的有状态组件中，我们时常会看到constructor(props){ super(props) }

我们一一来介绍一下这些到底是什么东西

#### constructor( ) ——构造方法
这是ES6对类的默认方法，通过 new 命令生成对象实例时自动调用该方法。并且，该方法是类中必须有的，如果没有显示定义，则会默认添加空的constructor( )方法。

#### super( ) ——继承

 在class方法中，继承是使用 extends 关键字来实现的。子类 必须 在 constructor( )调用 super( )方法，否则新建实例时会报错。

报错的原因是：子类是没有自己的 this 对象的，它只能继承自父类的 this 对象，然后对其进行加工，而super( )就是将父类中的this对象继承给子类的。没有 super，子类就得不到 this 对象。

所以在书写有状态组件的时候，正确的姿势应该是
``` js

constructor() {
  super();
  this.state = {searchStr: ''};
  this.handleChange = this.handleChange.bind(this);
}
```

那么有一个问题来了，为什么官方的例子都会加上 props 作为参数
``` js
constructor(props) {
  super(props);
  this.state = {searchStr: ''};
  this.handleChange = this.handleChange.bind(this);
}
```
#### 加与不加props的区别究竟在哪里呢？
以下来自stackoverflow的解释：

> What's the difference between “super()” and “super(props)” in React when using es6 classes?

> There is only one reason when one needs to pass props to super():

> When you want to access this.props in constructor.

> (Which is probably redundant since you already have a reference to it.)

只有一个理由需要传递props作为super()的参数，那就是你需要在构造函数内使用this.props


#### 如何使用有状态组件以及无状态组件进行开发
其实上面有说到，无状态组件的view层的变更其实是依据父组件传过来的props进行更新的，它是一个纯静态展示的组件。

而无状态组件存在这以下几个优势
* 1.代码整洁、可读性高
* 2.没有 this「由于使用的是箭头函数事件无需绑定」
* 3.没有生命周期的方法和状态更新，性能更好

虽然是这样，但是我们在开发过程中，也不可能全部使用无状态组件进行开发，而是使用有状态组件以及无状态组件相互嵌套开发

一般情况下，我会把需要用到生命周期函数的业务，数据逻辑处理等放在父组件的有状态组件里，然后通过props的形式，把数据以及方法传递给子组件的无状态组件进行展示


#### React 基础知识：有状态组件和无状态组件

注意：使用class关键字创建的组件，有自己的私有数据（this.state）和生命周期函数；

注意：使用function创建的组件，只有props，没有自己的私有数据和生命周期函数；

#### 什么情况下使用有状态组件？什么情况下使用有状态组件？

如果一个组件需要有自己的私有数据，则推荐使用：class创建的有状态组件；

如果一个组件不需要有自己的私有数据，则推荐使用：无状态组件；
React官方说：无状态组件，由于没有自己的state和生命周期函数，所以运行效率高

有状态组件和无状态组件之间的本质区别就是：有无state属性和生命周期函数

#### 组件中的props和state/data之间的区别

props中的数据都是外界传递过来的。state/data中的数据，都是组件私有的。

props中的数据都是只读的，不能重新赋值。state/data中的数据，都是可读可写的。
