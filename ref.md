某些情况下需要直接修改元素，要修改的子代可以是 React 组件实例，也可以是 DOM 元素。这时就要用到refs来操作DOM。

React 支持给任意组件添加特殊属性ref ，ref可以是一个字符串，也可以是一个属性接受一个回调函数，它在组件被加载或卸载时会立即执行。

[注意]在组件`mount`之后再去获取`ref`。`componentWillMount`和第一次`render`时都获取不到，在`componentDidMount`才能获取到。

版本16.3 之前，React 有两种提供 `ref`的方式：字符串和回调，因为字符串的方式有些问题，所以官方建议使用回调来使用 `ref`。而现在引入的`createRef API`，据官方说是一种零缺点的使用`ref`的方式，回调方式也可以让让路了。

##### React的ref有3种用法：

1. 字符串(已废弃)
2. 回调函数
3. React.createRef() （React16.3提供）

##### 1.字符串

最早的ref用法。

1.dom节点上使用，通过this.refs[refName]来引用真实的dom节点

```
<input ref="inputRef" /> //this.refs['inputRef']来访问
```

2.类组件上使用，通过this.refs[refName]来引用组件的实例

```
<CustomInput ref="comRef" /> //this.refs['comRef']来访问
```

**2. 回调函数**

回调函数就是在dom节点或组件上挂载函数，函数的入参是dom节点或组件实例，达到的效果与字符串形式是一样的，都是获取其引用。

回调函数的触发时机：

\1. 组件渲染后，即componentDidMount后
\2. 组件卸载后，即componentWillUnMount后，此时，入参为null
\3. ref改变后

1.dom节点上使用回调函数

```
<input ref={(input) => {this.textInput = input;}} type="text" />
```

2.类组件上使用

```
<CustomInput ref={(input) => {this.textInput = input;}} />
```

**3.React.createRef()**

在React 16.3版本后，使用此方法来创建ref。将其赋值给一个变量，通过ref挂载在dom节点或组件上，该ref的current属性将能拿到dom节点或组件的实例。

```
class Child extends React.Component{
    constructor(props){
        super(props);
        this.myRef=React.createRef();
    }
    componentDidMount(){
        console.log(this.myRef.current);
    }
    render(){
        return <input ref={this.myRef}/>
    }
}
```

注意：

1. ref在函数式组件上不可使用，函数式组件无实例，但是其内部的dom节点和类组件可以使用
2. 可以通过ReactDOM.findDOMNode()，入参是一个组件或dom节点，返回值的组件对应的dom根节点或dom节点本身
   通过refs获取到组件实例后，可以通过此方法来获取其对应的dom节点
3. React的render函数返回的是vDom(虚拟dom)

##### ref传递原理

当给 HTML 元素添加`ref 属性`时，`ref` 回调接收了底层的 DOM 元素作为参数。
 React 组件在加载时将 DOM 元素传入 ref 的回调函数**，在卸载时则会传入 null。

##### 类组件

当·`ref`属性用于使用 `class`声明的自定义组件时，`ref`的回调接收的是已经加载的 React 实例。

不管ref设置值是回调函数还是字符串，都可以通过ReactDOM.findDOMNode(ref)来获取组件挂载后真正的dom节点。

但是对于html元素使用ref的情况，ref本身引用的就是该元素的实际dom节点，无需使用ReactDOM.findDOMNode(ref)来获取，该方法常用于React组件上的ref。

Q：`this.refs` 和 `ReactDOM.findDOMNode`的区别

- ref添加到Compoennt上获取的是Compoennt实例，添加到原生HTML上获取的是DOM；
- `ReactDOM.findDOMNode`，当参数是DOM，返回值就是该DOM；当参数是Component获取的是该Component render方法中的DOM
- 二者主要区别在ref绑定在组件上的时候，`this.refs`获取到的是组件实例，`ReactDOM.findDOMNode`获取到的是dom节点。

###### 子组件调用父组件通过props

###### 父组件调用子组件的方法 - onRef

```
//Parent
import React,{Component} from "react";
import FileUpload from ".Fileload";
class Parent extends Component{
	handleClick = ()=>{
		alert(this.child.state.info)//通过this.child可以拿到child所有状态和方法
	}
	onRef = ref=>{
		this.child = ref;
		console.log(ref);//获取整个Child元素
	}
	render(){
	 return(
	 	<div>
	 		<FileUpload onRef={this.onRef} />
	 		<button onClick={this.handleClick} />
	 	</div>
	 )
	}
}
export defalut Parent;
```

```
//Child
import React,{Component} from 'React';
class Child extends React.Component{
	constructor(props){
		super(props);
		this.state={
			info:'快点击子组件按钮哈哈哈'
		}
	}
	componentDidMount(){
	  this.props.onRef(this);
	  console.log(this)//将child传递给this.props.onRef()方法
	}
	handleClick=()=>{
		this.setState({info:'通过父组件按钮取到子组件信息啦啦啦'})
	}
	render(){
		return <button onClick={this.handleChildClick}></button>*
	}
}
```

当在子组件中调用onRef函数时，正在调用从父组件传递的函数。this.props.onRef(this)这里的参数指向子组件本身，父组件接收该引用作为第一个参数：onRef=ref=>{this.child=ref},然后它使用this.child保存引用。之后，可以在父组件内访问整个子组件实例，并且可以调用子组件函数。

