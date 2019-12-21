---
title: 04-vuex中的mutations如何使用
date: 2018-07-30 17:36:17
tags:
  - Vuex
---
更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。
Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

### 1.其实在vuex中的mutations更加像是vue中的methods。编写也和methods类似

在store.js中编写方法
``` js
mutations:{ 
  reducePrice(state,payload){ 
    state.produce.forEach(reduce=>{ 
      reduce.price-=payload;
    })
  }
}
```
### 2.在组件当中使用mutations
``` js
this.$store.commit('reducePrice')
```

### 3.提交荷载（Payload）

你可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）：
在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：
``` js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}

store.commit('increment', {
  amount: 10
})
```