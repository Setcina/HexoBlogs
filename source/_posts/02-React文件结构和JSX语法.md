---
title: 02-React文件结构和JSX语法
date: 2018-08-22 19:41:22
tags:
  - React.js
---
题外话：为什么在同一文件目录还需要 './ ' 这样的方式选择文件，原因是因为在React.js以及Vue.js当中，引入一个Vue或者React，只需要import Vue/React from 'vue/react'。如果引用自己编写的模块（components）不加上' ./ '，Vue或者React会以为应用的是固有的自己架构中的模块~

### 1.JSX语法，首先必须
``` js
import React, { Component } from 'react'
引入react的模板
```

### 2.按需引入所需要的css或者JSX模板
``` js
import './App.css';  映入css
import User from './components/User'
```

### 3.结构语法
``` js
class App extends Component{	

  constructor(props) {
    super(props);
    this.state = {
      date: new Date()
    };
  }

  render(){	
    return(	
      <div className = "App-header">你好</div>
    )
  }

}


```
