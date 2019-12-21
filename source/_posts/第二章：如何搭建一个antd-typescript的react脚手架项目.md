---
title: 第二章：如何搭建一个antd+typescript的react脚手架项目
date: 2019-05-04 19:23:00
tags:
    REACT-CLI   
---
第一章已经使得我们可以使用react脚手架开始编写项目了，但是，其实我们深入考虑，我们还欠缺了很多东西，比如css样式，image图片，分离打包，文件压缩等。

现在，我们来解决一下这些问题

### 在项目中使用sass
```
// 首先安装依赖
npm install --save-dev style-loader css-loader sass-loader node-sass
```

#### 1.打开webpack.config.common.js
``` js
// 在rules里面，我们再添加一个对象，用来解决sass编译问题
{
    test: /\.(sa|sc|c)ss$/,
    use: [
      {loader:'style-loader'}
      {loader:'css-loader'},
      {loader:'sass-loader'}      
    ],
},
```

这样子我们就可以在项目里面使用sass了，但是，我门应该再可考虑一个问题，那就是css3浏览器兼容问题以及css，js分离打包的问题

#### 2.首先我们先安装一下依赖
``` js
npm install --save-dev postcss-loader mini-css-extract-plugin autoprefixer
```

#### 3.我们修改一下webpack.config.common.js里面的配置
``` js
引入mini-css-extract-plugin

const  MiniCssExtractPlugin  = require('mini-css-extract-plugin');

const Mode=process.env.NODE_ENV.trim();
const devMode= Mode ==='development' ? true:false;



{
    test: /\.(sa|sc|c)ss$/,
    use: [
      {
        loader: MiniCssExtractPlugin.loader,
        options: {
          hmr: devMode,
          //如果热更新没有执行的话，可以使用下面
          reloadAll:true
        },
      },
      { 
        loader:'css-loader'
      },
      { 
        loader:'postcss-loader',
        // 如果没有options这个选项将会报错 No PostCSS Config found
        options: {           
          plugins: (loader) => [
              require('autoprefixer')(), //CSS浏览器兼容
          ]
      }
      },  
      {
        loader:'sass-loader'
      }      
    ],
}

在plugins里面，添加
plugins: [
    // 创建一个在内存中生成的html插件
    new HtmlWebpackPlugin({
      title: 'React + Typescript Project',
      // 指定模板页面, 将来会根据指定的页面路径, 去生成内存中的页面
      template: paths.appHtml,
      inject: false
    }),
    //css和js分离打包
    new MiniCssExtractPlugin({
        //生产环境打包加上hash值
      filename: devMode ? '[name].css':'[name][hash:8].css',
      chunkFilename: '[id].css'
    }),
 ]
```

这样，我们就可以愉快的使用sass了，并且可以实现css以及js分离打包

### 我们再来解决图片的问题
``` js
npm install url-loader --save-dev
```

我们修改一下webpack.config.common.js里面的配置
``` js
{
    test: /\.(png|jpg|jpeg|gif)$/,
    use: [
      {
        loader: "url-loader",
        options: {
          //文件名称
          name: devMode ? "[name].[ext]":"[name]-[hash:5].min.[ext]",
          limit: 20000, // size <= 20KB
          //图片访问路径
          publicPath: devMode ? "./images/":"./static/images",
          //图片输出位置
          outputPath: "static/images/"
        }
      }
    ]
}
```

### antd的使用

前面我们安装了antd，但是我们一直没有使用，主要是使用antd，我们需要配置按需加载。接下来，我们解决一下这个问题

#### 1.在config/webpack下建立ts-import-plugin.js
``` js
首先安装依赖
npm install --save-dev ts-import-plugin

// 在ts-import-plugin.js写入
const tsImportPluginFactory = require("ts-import-plugin");

const ts_antd_options=[
    {
      libraryName: 'antd',
      libraryDirectory: 'es',
      style: true
    }
]

module.exports=()=>({    
    before:[
        tsImportPluginFactory(ts_antd_options)
    ]
})
```
#### 2.打开webpack.config.common.js
``` js
const tsImportPluginFactory = require('./ts-import-plugin');

修改ts-loader的配置
{
    test: /\.(ts|tsx)$/,  
    use:[{
      loader: "ts-loader",
      options:{  
        transpileOnly: true,
        getCustomTransformers:tsImportPluginFactory,
        compilerOptions: {
          module: 'es2015'
        }
      }
    }],
    exclude: /node_modules/
},
```

这样的话，我们基本上就完成了基于react-antd-typescript的脚手架搭建了。

但是，我们还少做了一些步骤。当我们在页面引用antd的组件的时候，运行react项目，会报错，报错的信息基本上是来源于less的问题。所以，我们继续解决一下这个问题
``` js
npm install less-loader less@2.7.3 --save-dev
// 这里说一下为什么需要指定less版本，由于在高版本的less上，运行antd组件的时候会报错，所以我们需要把less的版本降下来才可以
```

再修改一下webpack.config.common.js里面的内容
``` webpack.config.common.js
{ 
    test:/\.less$/,
    use:[ 
      {
        loader: MiniCssExtractPlugin.loader,
        options: {
          hmr: devMode,
          //如果热更新没有执行的话，可以使用下面的reloadAll
          reloadAll:true,
        },
      },
      { 
        loader:'css-loader'
      },
      { 
        loader:'less-loader'
      }
    ]
  },
```

这样，项目基本上就可以使用antd组件了







