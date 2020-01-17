---
title: 01-造react轮子并发布测试
date: 2020-01-17 11:12:13
tags:
  NPM
---

#### 什么叫做造轮子
看现在的软件发展趋势，越来越多的基础服务能够“开箱即用”、“拿来用就好”，越来越多的新软件可以通过组合已有类库、服务以搭积木的方式完成。这是趋势，将来不懂开发语言的人都可以通过利用现有软件组件快速构建出能解决实际问题的软件产品。

在这种趋势下，软件（服务）就慢慢演化为两极：

满足终端用户的应用类产品
解决软件产品通用问题的基础服务（组件）
比如你在自己的App中需要即时通信功能，完全可以使用融云、环信、网易云信等服务快速集成。

比如你想在自己的App中添加支付功能，完全可以使用Ping++或Pay++来解决诸多支付渠道的集成问题。

比如你想添加分享功能，ShareSDK、友盟SDK可以节省你很多时间。

比如你想做跨平台的游戏，使用Cocos2d-x远比自己在Android、iOS上从底层从OpenGL ES干起要高效得多。

比如你想让你的网站支持更多用户更多并发，能够快速部署、迁移、规模复制，那完全可以借助阿里云、AWS、Azure等而没必要自己搞。

比如你想推送消息给用户，就可以用腾讯信鸽、极光、个推、百度云推送、友盟等。

……

类似的场景很多很多。这种趋势使得一部分厂商集中精力开发基础服务（组件），一部分企业集中精力解决用户需求。对基础服务（组件）厂商来讲，他通过解决更复杂的基础问题为其他厂商带来便利而盈利。对终端软件产品企业来讲，他通过解决用户问题给用户创造价值而盈利，从理论上讲，只要其产品从用户端或第三方获取的价值大于支出给基础服务厂商的价值，生意就可以做下去。


#### 如何使用react造轮子
首先要申明一下，我为什么是使用react，而不是用vue，或者原生的js去造轮子。主要是由于我工作的环境导致的，目前技术找比较偏向于react，并且在工作中，虽然可以经常使用一些类库来解决一些项目上的问题。但我考虑到，如果在不同项目，时长需要用到一个公共类库，而这个类库的业务又是相对独立的，那么将其抽离出来，成为一个单独的轮子去使用，明显要比复制黏贴更为合适。

先设想一下我们需要做些什么：
* 1. 使用webpack构建工具搭建脚手架
* 2. 安装项目打包需要的东西，一些常用的loader作为编译js或者css或者文件等
* 3. 将webpack的生产环境以及开发环境分开，以便我们用来预览以及推送到npm上
* 4. 本地预览以及链接到项目中使用

#### 1.构建一个基于webapck的脚手架（只简单构建，可依照自己的需求，进行优化处理）
```
打开终端，在自己喜欢的文件目录下按照以下步骤输入命令
1. mkdir reactDemoToNpm

2. cd reactDemoToNpm

3. npm init
一直回车就可以了
```

打开我们的项目目录结构，我们可以在package.json看到一些基本信息
``` js
{
  "name": "reactdemotonpm", // 项目名称
  "version": "1.0.0",  // 版本
  "description": "", // 项目描述
  "main": "index.js", // 项目入口
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "", // 作者
  "license": "ISC", // 开源许可证
}
```

#### 2.安装我们开发所需要的一些依赖项
##### 首先安装webapck
``` js
npm install -D webpack webpack-cli webpack-dev-server webpack-merge cross-env

// webpack-merge 用来把开发和生产的配置分开
// cross-env 用来把开发和生产环境

```

##### 安装babel用来处理jsx文件
``` js
npm install -D babel-loader @babel/core @babel/preset-env react react-dom 

并且新建.babelrc 文件,并输入
{
    "presets": [
        "@babel/react",
        "@babel/env"
    ]
}
```
##### 安装loader用来处理jsx文件
``` js
npm install -D css-loader style-loader react-hot-loader
```

##### 安装插件
``` js
npm install -D html-webpack-plugin uglifyjs-webpack-plugin clean-webpack-plugin
```

#### 3.配置webpack
``` js
新建文件夹src，然后新建一个index.js文件，输入
import React from 'react';
import './styles.css'
class Hello extends React.Component{
  render () {
    return (
      <h1>Hello world</h1>
    );
  }

}
export default Hello;
======================

index.js文件同目录下新建styles.css，输入
h1 {
    color: red;
}
======================

index.js文件同目录下新建indexBuild.js，输入
import Hello from './Hello';
export default Hello;
======================

index.js文件同目录下新建indexStart.js，输入
import React from 'react';
import ReactDOM from 'react-dom';
import Hello from './Hello';
import { AppContainer } from 'react-hot-loader'; 

const render = Component => {
  ReactDOM.render(
    <AppContainer>
      <Component />
    </AppContainer>,
    document.getElementById('app')
  )
}

render(Hello)
// Webpack Hot Module Replacement API
if (module.hot) {
  module.hot.accept('./Hello', () => {
    const NextApp = require('./Hello').default
    render(NextApp);
  })
}
======================

在根目录新建文件 webpack.commom.js  webapck.dev.js webpack.prod.js

// webpack.commom.js 
const webpack = require('webpack');
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const env = process.env.NODE_ENV;

// 开发环境的plugins
const pluginsDev = [
  new CleanWebpackPlugin(),
  new HtmlWebpackPlugin({
    title: 'Output Management',
    template: 'index.html'
  }),
  // 热模块更新
  new webpack.NamedModulesPlugin(),
  new webpack.HotModuleReplacementPlugin(),
]

// 生产环境的plugins
const pluginsProd = [
  new CleanWebpackPlugin(),
  new UglifyJSPlugin(),
]

module.exports = {
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: "index.js",
    // libraryTarget: 'commonjs2'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader' 
      },
      {
        test: /\.css$/, 
        use: [
          'style-loader',
          'css-loader',
        ]
      },
    ]
  },
  plugins: env === 'production' ? pluginsProd : pluginsDev,
}
======================

// webpack.dev.js
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
const path = require('path');

module.exports = merge(common, {
  entry: {
    app: [
      'webpack-dev-server/client?path=http://localhost:8989/__webpack_hmr',
      'webpack/hot/only-dev-server',
      path.resolve(__dirname, 'src/indexStart.js'),
    ]
  },
  devtool: 'inline-source-map',
  devServer: {
    contentBase: './dist',
    port: 8989,
    hot: true,
  }
});
======================

// webpack.prod.js
const merge = require('webpack-merge');
const path = require('path');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  entry: {
    app: [
      path.resolve(__dirname, 'src/indexBuild.js'),
    ]
  },
  output: {
    path: path.resolve(__dirname, 'lib'),
    filename: "index.js",
    // npm常用的js格式
    libraryTarget: 'commonjs2'
  },
});
```

#### 4.修改package.json
``` js
 "name": "reactdemotonpm01",
  "version": "1.0.0",
  "description": "",
  "main": "lib/index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "cross-env NODE_ENV=development webpack-dev-server --hotOnly --config webpack.dev.js",
    "build": "cross-env NODE_ENV=production webpack --config webpack.prod.js"
  },
  "author": "",
  "license": "ISC",
  
  
  将对象main设置为lib/index.js意味着发布npm模块之后，会默认在lib/index.js下面查找我们发布的react模块
```

#### 声明

首先，其实我不应该在这里使用多个模块，尤其是我分开了dev-server以及生产环境。其实，我们发布的时候，应该就只有生产环境的，但是我考虑到我们需要在本地做开发调试，所以把dev-server的webpack设置也加入进来了。我们在造轮子的时候，也可以这样做，但是在发布的时候，应该去除这部分的代码


#### 5.如何测试
首先我们确定npm run start是可以在本地进行调试的，那么，如何在其他项目进行调试呢？？？

``` js
首先打包npm run build 

执行npm link 之后，会输出以下内容，这个时候表示已经把reactdemotonpm这个模块，发布到你本机的node模块中。

/Users/xingexiaer/.nvm/versions/node/v9.6.0/lib/node_modules/reactdemotonpm -> /Users/xingexiaer/Desktop/SetcinaProjectDemo/reactDemoToNpm
```

#### 6.项目测试

讲道理，上面的操作其实已经把我们的reactdemotonpm轮子公开到本地的node_module中，我们已经可以使用 <Hello /> 愉快的使用了。但是，如果出现找不到模块的情况，我们可以执行 npm link reactdemotonpm 进行连接


#### 7.推送到npm
1. 首先去npm官网注册账号

2. 打开reactdemotonpm项目，将我之前提到的，dev-server得配置去除

3. npm login 输入用户名和密码（最好要验证邮箱，在注册的时候）

4. npm publish


#### 8.项目中映入
最后的最后，在自己的项目中 npm install --save ****(对应的npm包名)，就可以正常使用了。

后面会把代码提到github，请关注以下链接以及博客系统的更新

下面是广告时间:

[项目地址](https://github.com/Setcina/reactDemoToNpm)


[我的github](https://github.com/Setcina)

[我的博客](setcina.github.io)










