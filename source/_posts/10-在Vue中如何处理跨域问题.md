---
title: 10-在Vue中如何处理跨域问题
date: 2018-10-23 15:23:25
tags:
  - Vue.js
---

 在项目开发的时候，接口联调的时候一般都是同域名下，且不存在跨域的情况下进行接口联调，但是当我们现在使用vue-cli进行项目打包的时候，我们在本地启动服务器后，比如本地开发服务下是 http://localhost:8080 这样的访问页面，但是我们的接口地址是 http://xxxx.com/save/index 这样的接口地址，我们这样直接使用会存在跨域的请求，导致接口请求不成功，因此我们需要在打包的时候配置一下，我们进入 config/index.js 代码下如下配置即可：

 ``` js

 // 静态资源文件夹
    assetsSubDirectory: 'static',

    // 发布路径
    assetsPublicPath: '/',

    // 代理配置表，在这里可以配置特定的请求代理到对应的API接口
    // 例如将'localhost:8080/api/xxx'代理到'www.example.com/api/xxx'
    // 使用方法：https://vuejs-templates.github.io/webpack/proxy.html
    proxyTable: {
      '/api': {
        target: 'http://xxxxxx.com', // 接口的域名
        // secure: false,  // 如果是https接口，需要配置这个参数
        changeOrigin: true, // 如果接口跨域，需要进行这个参数配置
        pathRewrite: {
          '^/api': ''
        }
      }
    },

 ```
Vue请求

``` js
fetch( '/apis/health/list',{  
  method:'post',
  headers:{ 
    "Content-type":'application/json',
    token:'********'
  },
  body:JSON.stringify({ 
    username:'***',
    password:'123321'
  })
})
.then(result=>{ 
  return result.json();
})
.then(data=>{ 
  console.log(data)
})
```