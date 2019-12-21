---
title: 03-如何高效使用vuex来获取数据
date: 2018-07-26 08:23:38
tags:
  - Vuex
---

### 我们需要注意的是vuex作为一个状态管理器，需要再state中建立数据

首先创建一个store文件夹并在里面创建store.js，构建好组件所需要的数据，然后，通过getters这个属性，去构造方法。
<br>
什么是getter
<br>
有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数：
``` js
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```
如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。例子如下

``` js
//引入vue以及vuex
import Vue from 'vue'
import Vuex from 'vuex'

//引用vuex
Vue.use(Vuex)
//建立数据store
export const store = new Vuex.Store({	
  state:{ 
    produce:[ 
      {name:'汉堡',price:'10'},
      {name:'薯条',price:'4'}
    ]
  },
  getters:{ 
    originState(state){ 
      return state.produce
    }
  }
})

```

在这里，我们构造了一个方法originState，那么，我们也应该如何使用
### 在组件中，我们需要调用到计算属性，用来给组件以及store.js构建一个通道（computed）
``` js
export default{ 
  name :'one',
  computed:{  
    price(){  
      return this.$store.getters.originPrice
    }
  } 
}
```

注意写法 this.$store.getters.originPrice

最后，在组件中使用数据
{{ price }}