---
title: 02-如何在Vue.js使用css
date: 2018-07-22 18:19:45
tags:
  - Vue.js
---

## 关于css部分
在vue-cli中使用sass、less来编写css样式，步骤十分简洁，因为vue-cli已经配置好了sass、less，我们要使用sass或者less直接下载两个模块，然后webpack会根据 lang 属性自动用适当的加载器去处理。以下我们来了解一下各css应该如何使用

#### css
1.直接上手写样式即可，使用css规则即可。
``` bash
<style lang="css">  
  body{ 

  }
</style> 
或者

<style lang="css" scoped>  
  body{ 

  }
</style> 
```

2.应用外部css文件的写法
``` bash
<style lang="css">  
  @import './index.css'  
</style> 
或者
<style lang="css" src="./index.css"></style> 
```

### 如何使用Scss
1.安装Sass模块
``` bash
npm install node-sass --save-dev  
npm install sass-loader --save-dev
```

2.在组件的style部分使用内联写法
``` bash
<style lang="scss" scoped>     //在这个部分添加lang="scss"
 //sass样式  
</style> 
```

3.引用sass外部文件的写法。
``` bash
<style lang="scss" src="./index.scss"></style>
```

### 如何使用Less
1.安装Less模块
``` bash
npm install less --save-dev  
npm install less-loader --save-dev  
```

2.在组件的style部分使用内联写法
``` bash
<style lang="less" scoped>     //在这个部分添加lang="less"  
 //less样式  
</style>   
```

3.引用less外部文件的写法。
``` bash
<style lang="less" src="./index.less"></style>
```






