---
title: 12-如何高效使用vuex来获取数据
date: 2018-07-26 08:23:38
tags:
  - Vue.js
---

### 我们需要注意的是vuex作为一个状态管理器，需要再state中建立数据

在store.js中，构建好组件所需要的数据，然后，通过getters这个属性，去构造方法。
``` bash
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
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