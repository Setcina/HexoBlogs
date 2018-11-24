---
title: 05-React-传值方式
date: 2018-09-13 16:23:32
tags:
  - React.js
---

### 父组件向子组件传值
通过props来传值，这种应用，很多时候我们某个组件在不同的地方用到，但是就只是内容不一样，这样在调用组件就是父组件的时候给各自自己的值就好

``` js
//子组件
class Es6cComponent extends React.Component{
    constructor(props){
        super(props);
    }
    render(){
        return(
            <div>
              <div>{this.props.nameall}</div>
            </div>
        )
    }
}
//父组件
class App extends React.Component{
    render(){
        return(
            <div>
                <Es6cComponent nameall="abc"/>
            </div>
        )
    }
}
```

### 子组件向父组件传值（回调函数）
