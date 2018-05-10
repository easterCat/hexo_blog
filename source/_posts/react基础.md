---
title: '''react基础'''
date: 2018-05-09 17:39:55
tags: react
---


[react官网](https://reactjs.org/docs/hello-world.html)

## 组件
#### 1. 无状态组件
  - 在React中，组件的名字必须用大写字母开头，而包含该组件定义的文件名也应该是大写字母(便于区分，也可以不是)。
  - 无状态组件是纯展示组件，仅仅只是用于数据的展示，只根据传入的props来进行展示，不涉及到state状态处理，通过函数式的方式来创建一个无状态函数式组件(大多数组件都是无状态组件，通过简单的组合可以构建成其他的组件，最后合并成一个大的应用)。
  - 无状态函数式组件是一个只带有render方法的组件，通过函数形式或者箭头函数形式创建，该组件无state状态。

```
import React from "react";

//创建方式一,相当于es5的函数声明的方式创建
 function NoState (props) {
     return (
         <div>this is NoState Component</div>
     )
 }
```

```
//创建方式二,相当于es5的函数表达式的方式创建
const NoState = (props) => {
    return (
        <div>this is Nostate Component</div>
    )
}

export default NoState
```
--------------------------------------------------------------------------------
- 无状态函数式组件没有组件实例化的过程，成为一个render方法的函数来执行，减少分配的内存，使整体渲染性能得到提高，因此展示数据的组件优先选择这种方式。
- 无状态组件没有实例化得过程，因此无法访问组件this中的对象
- 无状态组件不需要组件生命周期管理和状态管理，底层在实现这种形式的组件的时候不会实现组件的生命周期方法，所以无状态组件不能参与组件生命周期管理
- 无状态组件只能访问传入的props，同样的props会得到同样的渲染结果

<!-- more -->

当我们的组件开始有逻辑处理，之前的那种方式胜任不了时索要采取的一种形式，通过继承react的Component对象而来
代码的render方法，则是Component中，专门提供的用来处理jsx模板的方法。
第一种方式不同的是，我们接收传入进来的参数，使用的是this.props，第一种方式将props放置于函数参数中，而这种方式则是将props挂载与实例对象上，因此会有所不同
```
// helloWorld.jsx
import React, {Component} from 'react';
class HelloWorld extends Component {
    clickHander = () => {
        console.log(this.props);
        console.log(this.props.name);
    }

    render() {
        return (
            <div onClick={this.clickHander}>{ this.props.name } say: Hello World!</div>
        )
    }
}
export default HelloWorld;
```


## 组件之间的交互
#### 父组件与子组件之间的交互
1. 父组件修改子组件，只需要修改传入的props属性
2. 子组件修改父组件，需要父组件向子组件传递一个函数，该函数在父组件中定义，在子组件中触发执行
子组件与子组件之间的交互
3. 通过影响共同的父组件来进行交互



```
state = {
    switch: 0,
    name: this.props.name1
}
clickHander = () => {
    const {name1, name2} = this.props;
    if (this.state.switch === 0) {
        this.setState({
            switch: 1,
            name: name2
        })
    } else {
        this.setState({
            switch: 0,
            name: name1
        })
    }
};
render() {
    return (
        <div onClick={this.clickHander}>hello world !{this.state.name}</div>
    )
}
```

先来说说state相关的基础知识。首先了解ES6 class语法的同学都应该知道，当我们通过这种方式来写的时候，其实是将state写入了构造函数之中。

state = {} // 等同于ES5构造函数中的this.state = {}
在对象中，我们可以通过this.state的方式来访问state中所存储的属性
setState接收一个对象，它的运行结果类似于执行一次assign方法。会修改传入的属性，而其他的属性则保持不变
react赋予state的特性，则是当state被修改时，会引起组件的一次重新渲染。即render方法会重新执行一次。也正是由于这个特性，因此当我们想要改变界面上的元素内容时，常常只需要改变state中的值就行了
而setState也有一个非常重要的特性，那就是，该方法是异步的。它并不会立即执行，而会在下一轮事件循环中执行
// 假设state.name的初始值为Tom，我们改变它的值this.setState({    name: 'Jason'})// 然后立即查看它的值console.log(this.state.name) // 仍然为Tom，不会立即改变

### refs
react组件其实是虚拟DOM，因此通常我们需要通过特殊的方式才能拿到真正的DOM元素。大概说一说虚拟DOM是个什么形式存在的，它其实就是通过js对象的方式将DOM元素相关的都存储其实，比如一个div元素可能会是这样
```
// 当然可能命名会是其他的，大概表达一个意思，不深究哈
{
    nodeName: 'div',
    className: 'hello-world',
    style: {},
    parentNodes: 'root',
    childrenNodes: []
    ...
}
```

而我们想要拿到真实的DOM元素，react中提供了一种叫做ref的属性来实现这个目的
```
import React, { Component } from 'react';
class HelloWorld extends Component {
    clickHander = () => {
        console.log(this.refs)
    } 
    render () {
        return (
            <div className="container" onClick={this.clickHander}>
                <div ref="hello" className="hello">Hello</div>
                <div ref="world" className="world">World</div>
            </div>
        )
    }
}
export default HelloWorld;
```
为了区分ES6语法中的class关键字，当我们在jsx中给元素添加class时，需要使用className来代替
我们在jsx中，可以给元素添加ref属性，而这些拥有ref属性的元素，会统一放在组件对象的refs中，因此，当我们想要访问对应的真实DOM时，则通过this.refs来访问即可。
当然，ref的值不仅仅可以为一个名字，同时还可以为一个回调函数，这个函数会在render渲染时执行，也就是说，每当render函数执行一次，ref的回调函数也会执行一次。
```
// src/helloWorld.jsx
import React, { Component } from 'react';class HelloWorld extends Component {
    clickHander = () => {
        console.log(this.refs)
    } refCallback = (elem) => {
        console.log(elem);
    } render () {
        return (
            <div className="container" onClick={this.clickHander}>
                <div ref="hello" className="hello">Hello</div>
                <div ref={this.refCallback} className="world">World</div>
            </div>
        )
    }
}export default HelloWorld;
```


### 组件生命周期
件的生命周期，指的就是一个组件，从创建到销毁的这样一个过程，react为组件的生命周期提供了很多的钩子函数

##### react组件有三种状态：
- Mounted：已经插入真实DOM
- Updating：正在被重新渲染
- Unmounted：已移出真实DOM
- 
##### 每个状态的处理函数
- will：函数在进入状态之前调用
- did：函数在进入状态之后调用

##### 组件第一次渲染完成的前后时刻，所谓的渲染完成，即组件已经被渲染成为真实DOM并插入到了html之中
  - componentWillMount 在首次渲染完成之前，此时可修改组件的state
  - componentDidMount 真实DOM渲染完成之后，该方法可通过this.getDOMNode()访问到真实的DOM元素，可以使用其他的类库来操作这个DOM

##### 组件属性(我们前面提到的props与state)更新的前后时刻
  - componentWillUpdate 接收到一个新的state或者props时，在重新render之前调用，此时不允许更新props和state
  - componentDidUpdate 组件完成更新之后调用，此时可访问新的DOM元素

###### 组件取消挂载之前(取消之后就没必要提供钩子函数了)
`componentWillUnmount`

###### 两个特殊的处理函数
  - `componentWillReceiveProps(object nextProps)`：组件接收到新的props时，在重新render之前调用,此时可以更改props和state。首先props发生改变->然后componentWillReceiveProps去判断是否需要重新渲染(`shouldComponentUpdate`)->如果不需要则继续running->如果需要则执行`componentWillUpdate`->渲染DOM树之后执行`componentDidUpdate`->进入running
  - `shouldComponentUpdate(nextProps, nextState)`(更新发生前立即被调用) 接收到一个新的state或者props时，在重新render之前调用，组件判断是否重新render前调用。首先state发生改变->判断是否需要重新渲染新的props和state(`shouldComponentUpdate`) -> 根据判断决定执行render过程还是继续·保持running状态

###### 三个统一调用的方法,用于组件初始化，获取默认属性和状态
  - getDefaultProps：作用于组件类，只调用一次，返回对象用于设置默认的props，对于引用值，会在实例中共享
  - getInitialState：作用于组件的实例，在实例创建时只调用一次，，用于初始化没个实例的state，可访问this.props
  - render：必选的方法，创建vistual DOM （1. 只能通过this.props和this.state访问数据 2. 可以返回null、false或任何React组件 3. 只能出现一个顶级组件（不能返回数组）4.不能改变组件的状态 5. 不能修改DOM的输出

###### 已挂载的方法
  - component.forceUpdate() 可以在任何已经挂载的组件上使用，当你知道某些深处的组件状态未通过this.setState而改变了的时候



>componentDidMount，组件第一次渲染完成之后调用的componentDidMount，既然是组件第一次渲染完成之后才会调用，也就是说，该函数在react组件的生命周期中，只会调用一次。而渲染完成，则表示组件已经被渲染成为真实DOM插入了html中。所以这时候就可以通过ref获取真实元素。在实际开发中，常常需要通过ajax获取数据，而数据请求的这个行为，则最适合放在componentDidMount中来执行。通常会在首次渲染改变组件状态(state)的行为，或者称之为有副作用的行为，都建议放在componentDidMount中来执行。主要是因为state的改动会引发组件的重新渲染。


![image](http://images2017.cnblogs.com/blog/1106982/201708/1106982-20170811224737742-1564011484.jpg)

## PropTypes
```
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // You can declare that a prop is a specific JS primitive. By default, these
  // are all optional.
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // Anything that can be rendered: numbers, strings, elements or an array
  // (or fragment) containing these types.
  optionalNode: PropTypes.node,

  // A React element.
  optionalElement: PropTypes.element,

  // You can also declare that a prop is an instance of a class. This uses
  // JS's instanceof operator.
  optionalMessage: PropTypes.instanceOf(Message),

  // You can ensure that your prop is limited to specific values by treating
  // it as an enum.
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // An object that could be one of many types
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // An array of a certain type
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // An object with property values of a certain type
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // An object taking on a particular shape
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // You can chain any of the above with `isRequired` to make sure a warning
  // is shown if the prop isn't provided.
  requiredFunc: PropTypes.func.isRequired,

  // A value of any data type
  requiredAny: PropTypes.any.isRequired,

  // You can also specify a custom validator. It should return an Error
  // object if the validation fails. Don't `console.warn` or throw, as this
  // won't work inside `oneOfType`.
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // You can also supply a custom validator to `arrayOf` and `objectOf`.
  // It should return an Error object if the validation fails. The validator
  // will be called for each key in the array or object. The first two
  // arguments of the validator are the array or object itself, and the
  // current item's key.
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

	