---
title: antd的Form动态校验规则
date: 2019-07-04 22:16:21
tags:
  Work Experience
---

### 序言
其实在Ant Design里面，表单的验证最主要是集中在动态校验规则中，但其实目前到手的工作项目中对于表单的实例少之又少。所以在这次的工作中，我也比较花时间的尝试了使用Form的动态校验规则。当然啦，尝试的话，就免不了有坑。顺手在这里总结一下

### getFieldDecorator
在动态校验规则中，getFieldDecorator几乎是真个RC-FORM中最重要的一部分了，它存在的意义是用来与表单进行双向绑定。它的使用方式如下：
``` js
this.props.form.getFieldDecorator(id, options)
// 实例
<Form.Item label="接收手机号码">
{getFieldDecorator(`${this.props.id}`, {
  initialValue: value,
  rules: [
    {
      required: true,
      max: 11,
      pattern: /^1[3456789]\d{9}$/,
      validator: this.checkValue,
    },
  ],
})(<Input placeholder="请输入手机号" />)}
</Form.Item>

// 1.在getFieldDecorator 包装的控件，表单控件会自动的添加value。可能这样说是不正确的，因为在getFieldDecorator之后，我们需要使用initialValue来动态绑定这个input的value值
// 2.rules这里面可以定义这个表单的触发时间，长度，必填项，以及提示语等等东西。在后面会讲到这个东西
```

那么在这个地方，我们抽出五个东西来讲一下，因为我觉得这是表单里面，最重要，也是最常使用到的一部分
#### pattern 正则
其实pattern是比较简单以及很好理解的，其实他就是一个正则校验而已，虽然没有什么好说，但它是我们规则检验中相当重要的一部分

#### validateTrigger 校验子节点值的时机
我们在上面代码rules中可以看到，我设置了一个pattern属性，这个的触发默认是在onChange的时候触发的，但是，当我们想要修改这个触发时机时，我们可以通过validateTrigger来设置。例如
``` js
<Form.Item label="接收手机号码">
{getFieldDecorator(`${this.props.id}`, {
  initialValue: value,
  rules: [
    {
      required: true,
      max: 11,
      pattern: /^1[3456789]\d{9}$/,
      validator: this.checkValue,
    },
    validateTrigger: 'onFocus'/'onBlur'
  ],
})(<Input placeholder="请输入手机号" />)}
</Form.Item>

```

#### validator 用于表单的动态校验
validator接收三个参数，分别是rule（这个代表着当前表单的基本信息，像是id，pattern等），value（当前表单的值），callback(必须被调用，这个后面会说为什么是必须的)；例如

``` js
checkValue = (rule, value, callback) => {  
  if(value.length > 0){ 
    if (rule.pattern.test( value )) { 
      callback()// 当检验通过之后，this.props.form.validateFields会调用到检验通过的值
    } else { 
      callback('请输入正确的手机号码');// 当检验不通过，会显示message等于‘请输入正确手机号码’的提示
    }
  }
  callback('手机号码不能为空')
}
<Form.Item label="接收手机号码">
{getFieldDecorator(`${this.props.id}`, {
  initialValue: value,
  rules: [
    {
      required: true,
      max: 11,
      pattern: /^1[3456789]\d{9}$/,
      validator: this.checkValue,
    },
    validateTrigger: 'onFocus'/'onBlur'
  ],
})(<Input placeholder="请输入手机号" />)}
</Form.Item>

```

#### this.props.form.validateFields 表单提交调用的方法
这个方法也会传入三个参数，分别是[fieldNames: string[]]，[options: object], callback(errors, values)。那么我们又该如何使用他呢，例如
``` js
submitValue=()=>{ 
  this.props.form.validateFields([name01,name02],(err,value)=>{ 
    // 前面是可以获取到的表单的值，我们一般会将它集成为一个数组，当你不传的时候，默认会获取全部表单内容
    // err是报错的信息，会返回一个object对象
    // 当点击提交的时候，会循环多次（这里的多次，看你的表单有多少内容）validator这个方法，将所有成功的value回调出来，返回一个object对象
    if (!err) {
        console.log(value);
        ...
      }

  })
}
<Form.Item label="接收手机号码">
{getFieldDecorator(`${this.props.id}`, {
  initialValue: value,
  rules: [
    {
      required: true,
      max: 11,
      pattern: /^1[3456789]\d{9}$/,
      validator: this.checkValue,
    },
    validateTrigger: 'onFocus'/'onBlur'
  ],
})(<Input placeholder="请输入手机号" />)}
</Form.Item>
<button onClick={ this.submitValue }>提交</button>
```

#### getFieldsValue 获取一组输入控件的值，如不传入参数，则获取全部组件的值
这是一个获取表单所有值得一个方法，它可以直接获取当前表单中所有的值，可以传入表单的id，如果不传则返回所有值，它返回也是一个object对象
使用如下
``` js
// 可以传入数组用来获取想要的表单值，返回的是对象组
const values = this.props.form.getFieldsValue();
```


