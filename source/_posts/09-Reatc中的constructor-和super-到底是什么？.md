---
title: 10-Reatc中的constructor()和super()到底是什么？
date: 2018-10-09 11:03:57
tags:
 - React.js
---

### constructor( )——构造方法

这是ES6对类的默认方法，通过 new 命令生成对象实例时自动调用该方法。并且，该方法是类中必须有的，如果没有显示定义，则会默认添加空的constructor( )方法。

ES5中，JavaScript是没有继承的写法的，因此，通过使用 prototype 来达到目的。举个例子：

``` js
//构造函数People
function People (name,age){
    this.name = name;
    this.age = age
}
People.prototype.sayName = function(){
    return '我的名字是：'+this.name;
}

//创建新的子类p1
let p1 = new People('harrisking',23);
```
但是在ES6中，可以通过class 来实现。举个例子：

``` js
class People{
  //构造方法constructor就等于上面的构造函数People
  constructor(name,age){
      this.name = name;
      this.age = age;
  }

  sayName(){
      return '我的名字是：'+this.name;
  }
}
//创建新的子类p1
let p1 = new People('harrisking',23);

```

上面两个例子等价。其实class方法完全就是对象原型的语法糖，效果是完全一样的。

### 原型链知识我们一起复习一下：
<img class="myimage" src="image01.png">
构造方法constructor( )其实就是构造函数本身。

### super( ) ——继承
在class方法中，继承是使用 extends 关键字来实现的。举例如下：
``` js
class People{
  constructor(name,age){
      this.name = name;
      this.age = age;
  }
  sayName(){
      return '我的名字是：'+this.name;
  }
}

class har extends People{
    constructor(name,age,sex){
        super(name,age);//调用父类的constructor(name,age)
        this.sex = sex;
    }
    haha(){
        return this.sex + ' ' + super.sayName();//调用父类的sayName() 
    }
}
```
上面的例子中，出现了 super( )，子类 必须 在 constructor( )调用 super( )方法。

因为子类是没有自己的 this 对象的，它只能继承自父类的 this 对象，然后对其进行加工，而super( )就是将父类中的this对象继承给子类的。没有 super，子类就得不到 this 对象，没有 this 对象而要对 this 进行处理，能不报错吗？
<br>
简单解释，就是在ES5的继承中，先创建子类的实例对象this，然后再将父类的方法添加到this上（ Parent.apply(this) ）。而ES6采用的是先创建父类的实例this（故要先调用 super( )方法），完后再用子类的构造函数修改this。
<br>
补充：
1.子类如果没有 constructor 方法，该方法会被默认添加，即任何的子类都有 constructor 方法，无论定义没定义，它就在那里，不离不弃。
2.在子类的构造函数 constructor( )中，只有调用 super( )方法之后，才可以使用 this关键字，否则会报错。
