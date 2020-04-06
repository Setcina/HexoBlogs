---
title: 02 React Hooks中父组件调用子组件的方法
date: 2020-04-06 16:57:13
tags:
  React Hooks
---

#### 1.子组件中的使用

首先，在子组件中，需要将方法暴露给父组件的useRef调用，需要使用 useImperativeHandle 这个方法，下面我们构造一个子组件
``` js
/* child子组件 */
// https://reactjs.org/docs/hooks-reference.html#useimperativehandle
import {useState, useImperativeHandle} from 'react';

// props子组件中需要接受ref
const ChildComp = ({cRef}) => {
    const [val, setVal] = useState();
    // 此处注意useImperativeHandle方法的的第一个参数是目标元素的ref引用
    useImperativeHandle(cRef, () => ({
        // changeVal 就是暴露给父组件的方法
        changeVal: (newVal) => {
          setVal(newVal);
        }
    }));
    ...
    return (
        <div>{val}</div>
    )
}
```

#### 2.父组件的使用
``` js
/* FComp 父组件 */
import {useRef} from 'react;

const FComp = () => {
    const childRef = useRef();
    const updateChildState = () => {
        // changeVal就是子组件暴露给父组件的方法
        childRef.current.changeVal(99);
    }
    ...
    return (
        <>
        {/* 此处注意需要将childRef通过props属性从父组件中传给自己 cRef={childRef} */}
        <ChildComp  cRef={childRef} />
        <button onClick={updateChildState}>触发子组件方法</button>
        </>
    )
}
```