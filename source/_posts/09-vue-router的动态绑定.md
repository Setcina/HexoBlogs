---
title: 09-vue-router的动态绑定
date: 2018-07-25 16:22:05
tags:
  - Vue.js
---

### 例如一份只带id的用户列表
在一份自带id的用户列表中，如何进行增删改查，vue-router里面的：id就起到很大的作用了
``` js
import Users from './components/Users'
export default [
  {path:'/users/:id',component:Users}
]
```
这里的id，我们可以理解为唯一标识，那么，如何将路由里面的id进行传导，这才是重点
我们可以再router-link里面这样来使用
``` js
<router-link v-bind:to="'/users/'+customer.id"></router-link>
```

这样，我们可以成功的将id的存到了vue-router里面
那么，我们又该如何利用vue-router里面的id，在我们页面进行响应呢，这里就要使用
``` js
methods:{	
  methods(id){	

  }
}
created(){	
在页面加载完成之前，执行方法，将我们的id代入在methods中
this.methods(this.$route.params.id)
}
```