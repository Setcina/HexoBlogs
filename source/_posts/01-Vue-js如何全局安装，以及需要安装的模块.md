title: 01-Vue.js如何全局安装，以及需要安装的模块
tags:
  - Vue.js
date: 2018-07-20 15:23:13
---
[Vue](https://cn.vuejs.org/v2/guide/) (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

## Quick Start
### 你可以直接使用生产版本的Vue.js
``` bash
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
或者
``` bash
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

### 更加高级一点的方法，你可以使用vue-cli
当然了，在全局安装vue-cli脚手架之前，你有必要先安装node.js，最好也顺手安装git以及webpack

``` bash
node.js安装地址：https://nodejs.org/en/
git安装地址：https://git-scm.com/download
webpack全局安装：npm install webpack -g
```

### 全局安装 vue-cli

``` bash
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm run dev
```

### 模块当然不能少~~~
``` bash
安装的依赖模块
路由
npm install vue-router --save-dev
resource
npm install vue-resource --save-dev
jq
npm install jquery --save-dev
less
npm install less --save-dev  
npm install less-loader --save-dev 
sass
npm install node-sass --save-dev  
npm install sass-loader --save-dev
```