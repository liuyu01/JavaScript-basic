#### react 迷惑的点

##### 为什么要引入 React

因为从本质上讲，JSX 只是为 `React.createElement(component, props, ...children)` 函数提供的语法糖。

##### 为什么要用 className 而不用 class

1.React 一开始的理念是想与浏览器的 DOM API 保持一直而不是 HTML，因为 JSX 是 JS 的扩展，而不是用来代替 HTML 的，这样会和元素的创建更为接近。在元素上设置 `class` 需要使用 `className` 这个 API

2.浏览器问题，ES5 之前，在对象中不能使用保留字。

3.解构问题，当你在解构属性的时候，如果分配一个 `class` 变量会出问题

##### 为什么属性要用小驼峰

因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 `camelCase`（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。

##### 为什么 constructor 里要调用 super 和传递 props

**为什么要调用 super**

其实这不是 React 的限制，这是 JavaScript 的限制，在构造函数里如果要调用 this，那么提前就要调用 super，在 React 里，我们常常会在构造函数里初始化 state，`this.state = xxx` ，所以需要调用 super。

**为什么要传递 props**

要是构造函数中调用了某个访问 `props` 的方法，那这个 bug 就更难定位了。**因此我强烈建议始终使用super(props)，即使这不是必须的**

##### 为什么调用方法要 bind this

你必须谨慎对待 JSX 回调函数中的 `this`，在 JavaScript 中，class 的方法默认不会[绑定](https://link.juejin.im?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_objects%2FFunction%2Fbind)`this`。如果你忘记绑定 `this.handleClick` 并把它传入了 `onClick`，当你调用这个函数的时候 `this` 的值为 `undefined`。

这并不是 React 特有的行为；这其实与 [JavaScript 函数工作原理](https://link.juejin.im?target=https%3A%2F%2Fwww.smashingmagazine.com%2F2014%2F01%2Funderstanding-javascript-function-prototype-bind%2F)有关。通常情况下，如果你没有在方法后面添加 `()`，例如 `onClick={this.handleClick}`，你应该为这个方法绑定 `this`。

##### React是如何处理事件的？

React的事件是合成事件。

简单的理解react如何处理事件的，React在组件加载（mount）和更新（update）时，将事件通过addEventListener统一注册到document上，然后会有一个事件池存储了所有的事件，当事件触发的时候，通过dispatchEvent进行事件分发。

##### 四种事件处理对比

1.直接bind this型

```
class Foo extends React.Component{
	handleClick(){
		this.setState({xxx:aaa})
	}
	render(){
		return(
			<button onClick={this.handleClick.bind(this)}>
				Click me
			</button>
		)
	}
}
```

优点：写起来顺手，一口气就能把这个逻辑写完，不用移动光标到其他地方。

缺点：性能不太好，这种方式跟 react 内部帮你 bind 一样的，每次 render 都会进行 bind，而且如果有两个元素的事件处理函数是同一个，也还是要进行 bind，这样会多写点代码，而且进行两次 bind，性能不是太好。(其实这点性能往往不会是性能瓶颈的地方，如果你觉得顺手，这样写完全没问题)

2.constructor 手动bind型

```
class Foo extends React.Component{
	constructor(props){
		super(props)
		this.handleClick = this.handleClick.bind(this);
	}
	handleClick(){
		this.setState({xxx:aaa})
	}
	render(){
		return(
			<button onCick={this.handleClick}>Click me</button>
		)
	}
}
```

优点：相比于第一种性能更好，因为构造函数只执行一次，那么只会bind一次，而且如果有多个元素都需要调用这个函数，也不需要重复bind，基本上解决了第一种的两个缺点。

缺点：没有明显的缺点，硬要说的话就是太丑了，然后不顺手

3.箭头函数型

```
class Foo extends React.Component{
	handleClick(){
		this.setState({xxx:aaa})
	}
	render(){
		return(
			<button onClick={(e)=>this.handleClick(e)}>
				Click me
			</button>
		)
	}
}
```

优点：顺手，好看

缺点：每次render都会重复创建函数，性能会差一点

4.利用class fields(类字段)型

```
class Foo extends React.Component{
	handleClick = ()=>{
		this.setState({xxx:aaa})
	}
	render(){
		return(
			<button onClick={this.handleClick}>click me</button>
		)
	}
}
```

优点：好看，性能好

##### 为什么要setState，而不是直接this.state.xx = oo

setState做的事情不仅仅只是修改了this.state的值，另外最重要的是它会触发React的更新机制，会进行diff，然后将patch部分更新到真实dom里。

如果你直接this.sate.xx=oo的话，state的值确实会改，但是改了不会触发UI的更新，那就不是数据驱动了。

##### setState是同步还是异步相关问题

1.setState时候同步还是异步？

执行过程代码同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的“异步”，所以表现出来有时是同步，有时是“异步”。

2.何时是同步，何时是异步呢？

只在合成事件和钩子函数中是“异步”的，在原生事件和setTimeout/setInterval等原生API中都是同步的。简单的可以理解为被React控制的函数里面就会表现出“异步”，反之表现为同步。

3.那为什么会出现异步的情况呢？

为了做性能优化，将 state 的更新延缓到最后批量合并再去渲染对于应用的性能优化是有极大好处的，如果每次的状态改变都去重新渲染真实 dom，那么它将带来巨大的性能消耗。

4.那如何在表现出异步的函数里可以准确拿到更新后的 state 呢？

通过第二个参数 `setState(partialState, callback)` 中的 callback 拿到更新后的结果。

或者可以通过给 setState 传递函数来表现出同步的情况：

```
this.setState((state) => {
	return { val: newVal }
})
```

5.那表现出异步的原理是怎么样的呢？

我这里还是用最简单的语言让你理解：在 React 的 setState 函数实现中，会根据 isBatchingUpdates(默认是 false) 变量判断是否直接更新 this.state 还是放到队列中稍后更新。然后有一个 batchedUpdate 函数，可以修改 isBatchingUpdates 为 true，当 React 调用事件处理函数之前，或者生命周期函数之前就会调用 batchedUpdate 函数，这样的话，setState 就不会同步更新 this.state，而是放到更新队列里面后续更新。

这样你就可以理解为什么原生事件和 setTimeout/setinterval 里面调用 this.state 会同步更新了吧，因为通过这些函数调用的 React 没办法去调用 batchedUpdate 函数将 isBatchingUpdates 设置为 true，那么这个时候 setState 的时候默认就是 false，那么就会同步更新。
