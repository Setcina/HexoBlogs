---
title: 04加载css以及配置package.json
date: 2019-04-10 22:00:46
tags:
    - WebPack
---
搭建网页，css是必不可少的，那么在webpack中如何使用css呢？

首先我们在src文件夹下，建立main.css，然后，我们可以给我们之前建立的index.html里面的body以及h1标签，给点样式

``` main.css
body{ 
  color: red;
  margin: 0;
  background: #444;
}
h1{ 
  height: 100vh;
  display: flex;
  color: white;
  align-items: center;
  justify-content: center;
  font-size: 5em;
  font-family: sans-serif;
  text-shadow: 0 0 20px black;
}
```

然后，我们修改一下main.js
``` js
require("./main.css")

// alert("Helo world!!!!")
```

然后，我们可以使用webpack-dev-server --config=config/webpack.dev.js
这个时候，其实终端会给我们报一些错误，错误里面会说到缺少style-loader以及css-loader

所以，我们需要安装依赖
``` js
npm install style-loader css-loader
```
安装完之后，我们需要修改webpack.dev.js里面的一些配置
在devServer同级下面，我们需要添加
```
module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          { loader: 'css-loader' },
        ]
      }
    ]
  }
```

但是呢，每次都需要执行webpack-dev-server --config=config/webpack.dev.js，这实在是太长了。
我们可以打开package.json这个文件

修改配置
``` js
"scripts": {
    "start": "webpack-dev-server --config=config/webpack.dev.js",
    "build": "webpack --config=config/webpack.dev.js"
  },
 //意思是，运行webpack-dev-server，它的运行配置是在config文件夹下面的webpack.dev.js
```

然后，我们只需要再终端使用npm start就可以使用了






