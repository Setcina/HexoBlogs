---
title: 07polyfill、preset、transform
date: 2019-04-21 16:29:39
tags:
 - WebPack
---
#### 话外题Polyfill是什么
ployfill是一个js库，主要抚平不同浏览器之间对js实现的差异。比如，html5的storage(session,local), 不同浏览器，不同版本，有些支持，有些不支持。Polyfill（Polyfill有很多，在GitHub上https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills），帮你把这些差异化抹平，不支持的变得支持了（典型做法是在IE浏览器中增加 window.XMLHttpRequest ，内部实现使用 ActiveXObject。）



如果我们使用了ES7，那么又该如何使得js可以编译成ES5呢，例如，我们使用async以及await，然后转为promise函数

首先，我们试着修改main.js里面的代码
``` js
require("./main.css")
require("./index.html")

var a = async () => {
  await console.log("hello world");
}
```

安装依赖 babel-plugin-async-to-promises
``` js
npm install babel-plugin-async-to-promises
```
修改babelrc里面的那内容
``` js
{
  "plugins": [
    "transform-es2015-arrow-functions",
    "async-to-promises"
  ]
}
```

终端输入  babel src/main.js


![image](https://cdn.nlark.com/yuque/0/2019/png/308891/1554429239542-37070d34-6737-4d50-a2d0-1840e2c06f04.png)


解释一：
Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。
举例来说，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。

解释二：
提示：polyfill指的是“用于实现浏览器不支持原生功能的代码”，比如，现代浏览器应该支持fetch函数，对于不支持的浏览器，网页中引入对应fetch的polyfill后，这个polyfill就给全局的window对象上增加一个fetch函数，让这个网页中的JavaScript可以直接使用fetch函数了，就好像浏览器本来就支持fetch一样。


指定类型，例如promise，polyfill可以再转化之前，帮我们预编译好
``` js
npm install babel-polyfill
```
修改webpack.dev.js里面的内容
``` js
//入口：有并且可以有多个
  entry: {
    //main: ['babel-polyfill', './src/main.js']
    main: ['core-js/fn/promise', './src/main.js']
  },
```

第二行的意思是 将js的方法里面的promise编译成es5



那么上述说了那么多，其实可以用环境变量来替代

首先我们安装
``` js
npm install babel-preset-env
```
修改babelrc里面的内容
``` js
{
  // "plugins": [
  //   "transform-es2015-arrow-functions",
  //   "async-to-promises"
  // ]
  "presets": [
    [
      "env",
      {
        "debug": true
      }
    ]
  ]
}
```

``` js
npm start
```

我们可以看到，它会自动给我们添加一些配置

![image](https://cdn.nlark.com/yuque/0/2019/png/308891/1554429299655-ad070f1f-ea71-4c21-8dd1-9bd4ae92728c.png)

然后，我们可以继续添加一些配置

npm install babel-plugin-transform-runtime
``` js
//解决promise对环境变量的污染以及解析最近的代码
  "plugins":[ 
    "transform-runtime"
  ]
```

06和07主要提供的是，如果在使用es6或者es7的情况下，解决各个浏览器的兼容问题，其实，目前有很多方案，我这份方案其实是比较零碎的，但是，这更加有利于初学者对于es6、7转换的了解






















