---
title: 05-React-传值方式
date: 2018-09-13 16:23:32
tags:
  - React.js
---

### 父组件向子组件传值
* 父组件通过属性进行传递，子组件通过props获取
``` js
//父组件
class CommentList extends Component{
    render(){
        return(
            <div>
               <Comment comment={information}/>
            </div>
        )
    }
}
//子组件
class Comment extends Component{
    render(){
        return(
            <div>
                <span>{this.props.comment}:</span>
            </div>
        )
    }
}

```
> 父组件ComentList引用子组件Comment时设置comment属性传递information，在在组件Comment中通过this.props.comment获取到information值

### 子组件向父组件传值
* 通过回调进行传递数据
``` js
//父组件
class CommentApp extends Component{
    constructor(){
        super();
        this.state = {comments:[]}
    }
    printContent(comment){
        this.setState({comment});
    }
    render(){
        return (
            <div>
                <CommentInput onSubmit={this.printContent.bind(this)} />
            </div>
        )
    }
}
//子组件
class CommentInput extends Component{
    constructor(){
        super();
        this.state = {
            usrName:'',
            content:''
        };
    }
    submit(){
        if(this.props.onSubmit){
            var {usrName,content} = this.state;
            this.props.onSubmit({usrName,content})
        }
        this.setState({content:''});
    }
    render(){
        return(
              <div>
               <div>
                   <span>用户名：</span>
                   <div className="usrName">
                       <input type="text" value={this.state.usrName} />
                  </div>
               </div>
                <div>
                    <span>评论内容：</span>
                    <div className="content">
                       <textarea value={this.state.content} />
                   </div>
                </div>
                <button onClick={this.submit.bind(this)}>submit</button>
            </div>
        )
    }
}

```
> 在父组件CommentApp中调用CommentInput通过属性onSubmit传入函数printContent，在子组件CommentInput中通过this.props.onsubmit调用函数通过参数形式进行值的传递

### 兄弟组件之间的传值
* 通过相同的父组件进行传递数据
 > 子组件A通过回调函数形式将数据传给父组件，接着父组件通过属性将数据传递给子组件B

* 通过发布/订阅进行传递
> 在子组件A中 commentDidMount函数中，发布事件，在子组件B中commentDidMount函数中对事件进行监听

使用context进行传递
``` js
class Parent extends Component(
   constructor(props) {
        super(props);
        this.state = { value: '' }
    }   
   getChildContext(){
        return {
          this.value = this.state.value;
        }  
    }

  render() {
        return (
            <div>
                    <Child1 />
                    <Child2 />
            </div>
        )
    }
}

class Child1 extends Component{
    change:function(){
        this.context.value = "heiheihei"
    }
    render() {
        return (
            <div>
                子组件一
                <p>{this.context.value}</p>
            </div>
        )
    }
}

class Child2 extends Component{
    render() {
        return (
            <div>
                子组件二
                <p>{this.context.value}</p>
            </div>
        )
    }
}
```
