---
title: 06babel转换
date: 2019-04-11 20:48:53
tags:
    - WebPack
---

#### 先了解一下为什么要安装babel
Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。
这意味着，你可以现在就用 ES6 编写程序，而不用担心现有环境是否支持。下面是一个例子。
``` js
// 转码前
input.map(item => item + 1);

// 转码后
input.map(function (item) {
  return item + 1;
});
```
上面的原始代码用了箭头函数，这个特性还没有得到广泛支持，Babel将其转为普通函数，就能在现有的JavaScript环境执行了。

首先安装  babel
``` js
npm install babel-core
npm install babel-plugin-transform-es2015-arrow-functions
npm install babel-cli
```

根目录下建立.babelirc文件
``` js
{
  "plugins": [
    "transform-es2015-arrow-functions"
  ]
}
```

修改main.js
``` js
require("./main.css")
require("./index.html")

var a = () => {
  console.log("hello world");
}
```
在终端执行命令 babel src/main.js


那么如何讲babel和webpack结合起来呢？

首先，我们安装babel-loader
``` js
npm instal babel-loader
```
然后我们修改一下webpack.dev.js里面的配置
``` js
module: {
    rules: [
      //js loaders
      {
        test: /\.js$/,
        use: [
          {
            loader: 'babel-loader'
          }
        ],
        //忽略文件夹
        exclude: /node_modules/
      },
```

npm start  运行一下，如果发现报错信息
@babel/core'
 babel-loader@8 requires Babel 7.x (the package '@babel/core'). If you'd like to use Babel 6.x ('babel-core'), you should install 'babel-loader@7

我们需要将babel-loader换成7.1.5的版本，因为babel-loader更新至8以后，不支持原来的配置了，所以我们手动降级一下
``` js
npm instal babel-loader@7.1.5
```
这样，我们就可以愉快的使用es6语法，而不需要去在意浏览器对于javascript的兼容问题



