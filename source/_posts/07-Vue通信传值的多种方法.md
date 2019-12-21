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


### 3.父组件往子组件传值props
#### 定义父组件，父组件传递 number这个数值给子组件，如果传递的参数很多，推荐使用json数组{}的形式
``` js
<template>
  <div class="parent">
    <Children number="888"></Children>
  </div>
</template>
<script>  
  import Children from 'components/children'

  export default{ 
    components:{  
      Children
    }
  }
</script>
```

#### 定义子组件，子组件通过 props方法获取父组件传递过来的值。props中可以定义能接收的数据类型，如果不符合会报错。

``` js
<template>
    <div class="children">
        {{number}} //显示父组件传过来的值
    </div>
</template>
<script>
    export default{ 
        props:{ 
            'number':[Number,String,Object],//限制父组件传递过来的数据类型，如果不符合就报销
            'string':[String]//可以传递多个值，逗号隔开
        }
    }
</script>
```

* 父子组件传值，数据是异步请求，有可能数据渲染时报错原因：异步请求时，数据还没有获取到但是此时已经渲染节点了
* 解决方案：可以在 父组件需要传递数据的节点加上  v-if = false，异步请求获取数据后，v-if = true

### 4.子组件往父组件传值，通过emit事件
``` js
<template>
    <div class="children">
        //子组件通过事件传值
       <button @click = "emitToParent">按钮点击传值个父组件</button>
    </div>
</template>
<script>
    export default{ 
        methods:{   
           emitToParent(){  
               this.$emit('child-event','数据')
           } 
        }
    }
</script>
```
``` js
<template>
    <div class="parents" >
        //用v-on绑定子组件的事件
       <Children v-on:child-event = 'parentEvent'></children>
    </div>
</template>

<script>
    import Children from 'components/children'
    export default{ 
        components:{    
            Children
        },
        methods:{   
           parentEvent(data){  
              console.log(data)//这里的数据就是子组件从父组件传过来的data
           } 
        }
    }
</script>

```


### 不同组件之间传值，通过eventBus（小项目少页面用eventBus，大项目多页面使用 vuex）
#### 定义一个新的vue实例专门用于传递数据，并导出

``` eventBus.js
import Vue from 'vue';
export default new Vue();

```
#### 定义传递的方法名和传输内容，点击事件或钩子函数触发eventBus.emit事件
``` js
<template>
    <div class="componentsA" >
        //用v-on绑定子组件的事件
       <button @click = "emitToB">传送数据给b</button>
    </div>
</template>

<script>
import eventBus from 'common/js/eventBus.js'

export default{ 
    methods:{   
        emitToB(){  
            eventBus.$emit('emitToA','data')
        }
    }
}
</script>
```

#### 接收传递过来的数据

``` js
<template>
    <div class="componentsB" >
        //用v-on绑定子组件的事件
       {{title}}
    </div>
</template>

<script>
import eventBus from 'common/js/eventBus.js'

export default{ 
    data(){ 
        tittle:''
    },
    mounted(){  
        this.getEventData()
    },
    methods:{   
        getEventData(){ 
            const that = this; //这个this是vue的实例，用that接受，用来与eventBus的vue区分
            eventBus.$on('emitToA',function(val){  
                that.title = val
            })
        }
    }
}
</script>

```