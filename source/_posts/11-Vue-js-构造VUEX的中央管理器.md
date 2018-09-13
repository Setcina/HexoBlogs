---
title: 11-Vue.js 构造VUEX的中央管理器
date: 2018-07-26 08:12:42
tags:
  - Vue.js
---

### 安装依赖
``` npm
npm install vuex --save-dev
```

### 在文件目录下建立store文件夹，并且建立store.js，如下图所示：
与static同级目录下建立的

### 在store.js中使用
``` js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

```

### 如何使用

``` bash
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
export const store = new Vuex.Store({	
  state:{ 

  }
})
```

### 在main.js中使用
``` js
import Vue from 'vue'
import App from './App.js'
import {store} from './store/store'

Vue.config.productionTop = false;

new Vue({ 
  store,
  el:'#App',
  router,
  compontents:{App}
  ....
})
``` 