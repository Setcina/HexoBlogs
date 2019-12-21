---
title: 05-如何使用redux实例
date: 2019-02-11 20:42:03
tags:
  - Redux
---
#### 1.首先，我们需要安装redux
``` js
npm install redux react-redux redux-thunk
```
这三个分别提供了   Provider   store   以及middleware

#### 2.打开App.js，引入需要的模块
``` js
import React, { Component } from 'react';
import logo from './logo.svg';
import Post from './components/Post'
import { Provider } from 'react-redux'
import { store } from './store'
import PostsForm from './components/PostsForm'
import './App.css';

class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <div className="App">
          <header className="App-header">
            <img src={logo} className="App-logo" alt="logo" />
            <h1 className="App-title">Welcome to React</h1>
          </header>
          <p className="App-intro">
            To get started, edit <code>src/App.js</code> and save to reload.
          </p>
          <PostsForm/>
          <Post/>
        </div>
      </Provider>
    );
  }
}

export default App;

```
Provider用于承载一整个store

引入Provider以及store，然后，需要将Provider作为标签引入，放在最外围的，并且映入store。这两步是关键

connect方法生成容器组件以后，需要让容器组件拿到state对象，才能生成 UI 组件的参数。

一种解决方法是将state对象作为参数，传入容器组件。但是，这样做比较麻烦，尤其是容器组件可能在很深的层级，一级级将state传下去就很麻烦。
React-Redux 提供Provider组件，可以让容器组件拿到state。

#### 3.在src的文件目录下，建立store.js

``` js
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk'
import rootReducer from './reducer/index'

const initState= {};

const middleWare = [thunk];

export const store = createStore(
  rootReducer,
  initState,
  applyMiddleware(...middleWare),
   )
```

* 1.applyMiddleware作用是将所有中间件组成一个数组，依次执行。而由于作用于更新的store.dispath接受的参数只能是对象而不是函数，所以redux-thunk可以使得store.dispath可以接受函数作为参数。
* initState空对象，可以理解为一个空的state
* rootReducer Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。
Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。所以Reducer需要拆分出来


#### 4.建立一个文件夹reducer，并且新建index.js

``` index.js
import {combineReducers} from 'redux'
import postReducer from './postReducer'


export default combineReducers({  
  posts:postReducer,
})
```
* Redux 提供了一个combineReducers方法，用于 Reducer 的拆分。你只要定义各个子 Reducer 函数，然后用这个方法，将它们合成一个大的 Reducer。
* 

#### 5.新建文件postReducer.js
``` js
//reducer的作用：返回新的状态
import {FETCH_POSTS,CREAT_POST} from '../actions/types'

const initialState = {  
  posts:[],
  creatPost:''
}

//ruducer拿到action分发过来的数据，判断是哪个类型的值，修改了那些值
export default function (state = initialState,action){  

  switch(action.type){ 
    case FETCH_POSTS:
      return {  
        ...state,
        posts:action.payload
      }

    case CREAT_POST:
      return {  
        ...state,
        creatPost:action.payload
      }

    default:
    return state;
  }
  
}
```
* reducer需要传递两个值，一个是state，另外一个是action
* switch里面的action.type其实是告诉reducer需要用哪个函数
* 书写格式是要return 一个新的state


#### 6.建立新文件夹action，并且建立type.js和postAction.js
``` js
export const FETCH_POSTS = "FETCH_POSTS";
export const CREAT_POST = "CREAT_POST"
```
* 作为类型区别，主要是为了分发到reducer里面使用

``` js
import {FETCH_POSTS,CREAT_POST} from './types'

// 分发操作，将数据分发到reducer里面
export const fetchPosts = () => dispatch =>{ 
   
  fetch('http://jsonplaceholder.typicode.com/posts')
  .then(Response=>{ 
    return Response.json()
  })
  .then(posts=>{
    dispatch({  
      type:FETCH_POSTS,
      payload:posts
    })
  })
  .catch(error=>{  
    
  })
  
}

export const creatPost = (post) => dispatch =>{  

  fetch('http://jsonplaceholder.typicode.com/posts',{ 
      method:'POST',
      headers:{ 
        "content-type":'application/json'
      },
      body:JSON.stringify(post)
    })
    .then(res=>res.json())

    .then(data=>{
      
      dispatch({  
        type:CREAT_POST,
        payload:data
      })
      
    })

}
```

* 将数据操作完之后，分发到reducer里面，reducer根据type类型，将值绑定成一个新的state，并且返回到一开始的reducer文件夹里面的index里面，形成一个新的state


#### 在需要应用到数据的地方使用redux
``` js
import React, { Component } from 'react'

import {connect} from 'react-redux'

import {fetchPosts} from '../actions/postAction'

 class Post extends Component {

  
  componentDidMount(){  

    //触发action
    this.props.fetchPosts()

  }

  componentWillReceiveProps(nextProps){ 
    if(nextProps.newPost){ 

      this.props.posts.unshift(nextProps.newPost);
    }
  }

  render() {

    const PostItem = this.props.posts.map(item=>{ 
      return(
        <div key={item.id}>
          <h3>{item.title}</h3>
          <p>{item.body}</p>
        </div>
      )
    })

    return (
      <div>
        <div>Posts</div>
        {PostItem}
      </div>
    )
  }
}

const mapStateToProps = state =>({ 
  posts:state.stateReducer.posts,
  newPost:state.stateReducer.creatPost
})

export default connect(mapStateToProps,{ fetchPosts })(Post)
```

[https://github.com/Setcina/redux-demo](https://github.com/Setcina/redux-demo)