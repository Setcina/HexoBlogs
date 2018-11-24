---
title: 07-Vue通信传值的多种方法
date: 2018-07-24 08:40:27
tags:
 - Vue.js
---

### 1.通过路由带参数进行传值

##### 两个组件 A和B,A组件通过query把orderId传递给B组件（触发事件可以是点击事件、钩子函数等）
``` js
this.$router.push({ path: '/conponentsB', query: { orderId: 123 } })//调整到B
```
##### 在B组件中获取A组件传递过来的参数
``` js
this.$route.query.orderId
```


### 2.通过设置Session Storage缓存的形式进行传递
两个组件A和B，在A组件中设置缓存orderData
``` js
const orderData = {'orderData':123,'price':88}
sessionStorage.setItem('缓存名称', JSON.stringify(orderData))
```

在B组件中获取A中设置的缓存
``` js
const dataB = JSON.parse(sessionStorage.getItem('缓存名称'))
```
* 在这里顺便介绍一下 Session Storage（程序退出销毁） 和 Local Storage（长期保存） 的区别
 > * localStorage和sessionStorage一样都是用来存储客户端临时信息的对象。

 > * 他们均只能存储字符串类型的对象（虽然规范中可以存储其他原生类型的对象，但是目前为止没有浏览器对其进行实现）。

 > * localStorage生命周期是永久，这意味着除非用户显示在浏览器提供的UI上清除localStorage信息，否则这些信息将永远存在。

 > * sessionStorage生命周期为当前窗口或标签页，一旦窗口或标签页被永久关闭了，那么所有通过sessionStorage存储的数据也就被清空了。

 > * 不同浏览器无法共享localStorage或sessionStorage中的信息。相同浏览器的不同页面间可以共享相同的 localStorage（页面属于相同域名和端口），但是不同页面或标签页间无法共享sessionStorage的信息。这里需要注意的是，页面及标 签页仅指顶级窗口，如果一个标签页包含多个iframe标签且他们属于同源页面，那么他们之间是可以共享sessionStorage的。



### 1.首先在子组件中注册方法

``` js
<template>
  <div>
    子组件:
    <span>{{childValue}}</span>
    <!-- 定义一个子组件传值的方法 -->
    <input type="button" value="点击触发" @click="childClick" />
  </div>
</template>
<script>
  export default {
    data () {
      return {
        childValue: '我是子组件的数据'
      }
    },
    methods: {
      childClick () {
        // childByValue是在父组件on监听的方法
        // 第二个参数this.childValue是需要传的值
        this.$emit('childByValue', this.childValue)
      }
    }
  }
</script>

//子组件中的注册方法需要使用this.$emit("A","B") 
//childByValue是作为注册的事件，需要在父组件中当成属性使用，this.childValue作为参数在父组件中使用
```

### 2.在父组件中使用
首先在指令调用的地方使用 v-on:A="C"
//事件调用了什么方法，这个方法带有参数，必须是 $event.  相当于接收了再子组件中的参数B
``` js
<template>
  <div>
    父组件:
    <span>{{name}}</span>
    <br>
    <br>
    <!-- 引入子组件 定义一个on的方法监听子组件的状态-->
    <child v-on:childByValue="childByValue"></child>
  </div>
</template>
<script>
  import child from './child'
  export default {
    components: {
      child
    },
    data () {
      return {
        name: ''
      }
    },
    methods: {
      childByValue: function (childValue) {
        // childValue就是子组件传过来的值
        this.name = childValue
      }
    }
  }
</script>
```