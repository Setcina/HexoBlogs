---
title: 05加载HTML和image
date: 2019-04-10 22:09:34
tags:
    - WebPack
---
首先我们将dist里面的index.html移动到src下，

修改main.js
``` main.js
require("./main.css")
require("./index.html")
```
然后我们再进去npm start，这个时候会报错，提示我们缺少相应的模块

我们首先安装 html-loader extract-loader file-loader这三个模块
``` js
npm install html-loader extract-loader file-loader
```
再一次
修改webpack.dev.js的配置
``` js
// html loader
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          { loader: 'css-loader' }
        ]
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "file-loader",
            options: {
              name: "[name].html"
            }
          },
          //将我们的index.html和bundle.js进行区分
          {
            loader: "extract-loader"
          },
          {
            loader: "html-loader"
          }
        ]
      }
    ]
  }

不懂webpack-loader的使用，可以到官方文档查看。而loader的存在本质上是为了将一些sass，tsx文件，转码编译成浏览器可以识别的js

这几个loader的意思是，编译的时候，html-loader找到了我们的html文件，使用了extract-loader帮助我们把js和html文件打包分离，最后使用file-loader给文件进行命名
```
再一次npm start ，我们可以发现已经可以正常的运行了

那么，如何使用图片呢？

首先，我们修改下一下index.html里面的内容
``` html

<div id='root'>
    <h1>Hello World</h1>
    <img src="./images/link.png" alt="">
</div>
```
并且在src下面建立一个images的文件夹，放入link.png这张图片

修改webpack.dev.js里面的内容
``` js
    再添加
    //image loader
      {
        test: /\.(jpg|gif|png)$/,
        use: [
          {
            loader: 'file-loader',
            options: {
              name: "images/[name].[ext]"
              //name: "images/[name]-[hash:8].[ext]"
            }
          }
        ]
      }
```

然后，让我们修改一下css样式
```
body{ 
  color: red;
  margin: 0;
  background: #444;
}

#root{  
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-flow: column;
}
img{  
  border-radius: 100%;
  width: 300px;
  box-shadow: 0 0 20px black;
}
h1{ 
  
  color: white;
  font-size: 5em;
  font-family: sans-serif;
  text-shadow: 0 0 20px black;
}
```
重新npm start 看到页面

![页面展示情况](https://cdn.nlark.com/yuque/0/2019/png/308891/1554424968568-d1a1f597-8047-40a3-9d81-b4cb06f623ca.png)














