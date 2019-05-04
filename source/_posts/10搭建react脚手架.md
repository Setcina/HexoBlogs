---
title: 10搭建react脚手架
date: 2019-04-21 18:48:33
tags:
    - WebPack
---
安装react以及react-dom
``` js
npm install react react-dom
```
在src下面建立app.js

修改main.js里面的内

``` js
require("babel-runtime/regenerator")
//让es6编译成浏览器可读的es5
require("babel-register")
//引入app.js
require("./app")

require("webpack-hot-middleware/client?reload=true")
require("./main.css")
require("./index.html")
```

app.js
``` js
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>This is React.js</h1>,
  document.getElementById("app")
)
```

``` js
npm install @babel/preset-react
针对所有 React 插件的 Babel 预设，例如将 JSX 转换为函数
```

打开babelrc文件，添加配置

``` js
"presets": [
    [
      "env",
      {
        "targets": {
          // 支持的浏览器的版本
          "browsers": [
            "last 2 versions"
          ],
          "node": [
            "v9.2.1"
          ]
        },
        "debug": true
      }
    ],
    "babel-preset-react"
  ],
```

课外知识
* babel-loader：使用 Babel 转换 JavaScript依赖关系的 Webpack 加载器
* babel@babel/core：即 babel-core，将 ES6 代码转换为 ES5
* babel@babel/preset-env：即 babel-preset-env，根据您要支持的浏览器，决定使用哪些 transformations / plugins 和 polyfills，例如为旧浏览器提供现代浏览器的新特性
* babel@babel/preset-react：即 babel-preset-react，针对所有 React 插件的 Babel 预设，例如将 JSX 转换为函数
* **注：babel 7 使用了 @babel 命名空间来区分官方包，因此以前的官方包 babel-xxx 改成了 @babel/xxx







