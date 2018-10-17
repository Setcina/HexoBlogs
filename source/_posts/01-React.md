---
title: 01-React全局搭建脚手架
date: 2018-08-22 19:34:31
tags:
  - React.js
---
### 1.全局安装

``` bash
npm install -g create-react-app
```

### 2.添加一个名为my-app的react的项目

``` bash
create-react-app my-app
```

### 3.打开my-app
``` bash
cd my-app
```
### 4.运行
``` bash
npm start
```

### 5如果是在现有的项目安装react
``` js
//首先初始化
npm init
//安装react以及react-dom
npm install react react-dom
```

### 6如何使用？
``` js
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```


