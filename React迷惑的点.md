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

##### JSX是什么？

全称：javascript and XML

定义：可扩展（自定义）标记性语言，基于javascript，融入了XML，我们可以在js中书写xml，使用JSX可以很好的描述UI在页面中应该呈现它应有的交互形式

```
ReactDOM.render(要渲染的组件，组件要挂载的位置)
```

JSX其实就是javascript对象，是用来描述UI结构信息的。

JSX是JavaScript语言的一种语法扩展，长得像HTML，但并不是HTML，附加了原生HTML标签不具备的能力，例如：自定义属性，以及后续的组件传值

UI界面显示什么样，取决于JSX对象结构，换句话说，取决于render()函数里面的return关键字后面返回的JSX内容结构

引入React.js库是为了解析识别JSX语法，同时创建虚拟DOM，而引入react-dom是为了渲染组件，将组件挂载到特定的位置上，同时将虚拟DOM转换为真实DOM，插入到页面中

使用扩展运算符...在JSX中传递整个props对象

JSX中添加属性的命名方式是camelCase 

React中定义组件，组件名称的首字母大写。

##### React的工作方式及优点

使用React的方式，就可以避免构建这样复杂的程序结构，无论何种事件，引发的都是React组件的重新渲染，它只会修改数据变化的DOM部分，并不需要去关心怎么去操作DOM

![img](https://user-gold-cdn.xitu.io/2019/7/26/16c2c557affceade?imageslim)



构建组件，本质上就是编写javascript函数，而组件中最重要的是数据，在React中数据分两种：props和state。当定义一个组件时，它接收任意的形参（即props），并用于返回描述页面展示内容的React元素

无论props还是state，当它们任何一个发生改变时，都会引发render函数的重新渲染

一个UI组件所渲染的结果，就是通过props和state这两个属性在render方法里面映射生成对应的HTML结构

##### Props

当通过函数声明或class自定义一个组件时，它会将JSX所接受的属性转换为以对象传递给该定义时的组件，这个接收的对象就是props。props就是组件定义属性的集合，它是组件对外的接口，由外部通过JSX属性传入设置（也就是从外部传递给内部组件的数据）

在React中，你可以将prop类似于HTML标签元素的属性。而在React中，Prop的属性值类型可以任何数据类型。当然如果是非字符串数据类型，在JSX中，必须要用{}把prop值包裹起来。

父组件向子组件传值是通过设置JSX属性的方式实现的，而在子组件内部获取父组件数据是通过this.props来获取的。也可以这么认为，props就是对外提供的数据接口。

##### State

state代表的是当前组件的内部状态，可以把组件看成一个‘状态机’，它是能够随着时间变化的数据，更多的是应当在实现交互时使用，根据状态state的改变呈现不同的UI展示。

当需要记录组件自身数据变化时，想要使组件具备交互的能力，那么需要有触发该组件基础数据模型改变的能力，那么此时就需要使用state

通过在React中封装的事件，例如;onChange、onClick、onKeyDown、onFocus、onBlur等这些事件类型里面绑定事件方法内的setState都是异步的。

如果是React控制的事件处理程序以及在它的钩子函数内调用setState，它不会同步的更新state

在这里，只需要知道，对于在React中的JSX绑定的事件处理函数中调用setState方法是异步的就可以了。

如果你需要基于当前的state来计算出新的值，那么setState就应该传递一个函数，而不是一个对象，它可以确保每次调用的都是使用最新的state。

##### 多个setState调用会合并处理

当在事件处理方法内多次调用setState方法时，render函数只会执行一次，并不会导致组件的重复渲染，因为React会将多个this.setState产生的修改放在一个队列里面进行批量延迟处理，所以从这点上讲，React设计这个setState是非常高效的，结合了函数式编程，不用考虑性能的问题。

##### 那么究竟什么样的数据属性可以视为状态？

状态（state）应该是会随着时间产生变化的数据，当更改这个状态（state），需要更新组件的UI，就可以将它定义成state，更多是在实现页面的交互时使用的

写静态，没有任何交互页面时，用props进行数据的填充

##### 无状态的组件（UI组件/函数式组件）

可以用纯粹的函数来定义，所谓纯函数，只有输入和输出，无状态，无生命周期钩子函数，只是用作于接受父组件传来的props值渲染生成DOM结构，无交互，无逻辑层的数据展示

无状态（函数式）组件，在性能上是最高效的，开销很低，因为没有那些生命周期函数

##### props与state的灵魂对比

相同点：都是组件内的数据,是一普通的javascript对象,都是用来保存信息的,这些信息可以控制组件的形态

不同点：

props是由父组件传入的(类似形参),用于定义外部组件的接口,是React组件的输入,它是从父组件传递给子组件的数据对象,在父(外部)组件JSX元素上,以自定义属性的形式定义，传递给当前组件,而在子组件内部,则以this.props或者props进行获取

props只具备读的能力,不能直接被修改,如果想要修改某些值,用来响应用户的输入或者输出响应,可以借用React内提供的setState函数进行触发,并用state来作为替代

state是当前组件的内部状态,它的作用范围只局限于当前组件,它是当前组件的一个私有变量.用于记录组件内部状态的,如果组件中的一些数据在某些时刻发生变化,或者做一些页面逻辑交互时,需要更新UI,这个时候就需要使用state来跟踪状态(例如控制一元素的显示隐藏来回切换等状态),它由组件本身管理,可以通过setState函数修改state

##### 向事件处理程序中传递参数

以下两种方式都可以向事件处理函数传递参数

```
<button onClick={this.handleBtnDelete.bind(this,id)}>删除</button>
或者
<button onClick={(e)=>this.handleBtnDelete(id,e)}>删除</button>
```

使用箭头函数，React的事件对象会被作为第二个参数传递，而且也必须显示的传递进去

而通过bind的方式，事件对象以及更多的参数将会被隐式的传递进去
