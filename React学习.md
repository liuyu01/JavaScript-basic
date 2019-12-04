##### 从头开始打造工具链

一组JavaScript构建工具链通常由这些组成：

- 一个package管理器，比如Yarn或npm。它能让你充分利用庞大的第三方package的生态系统，并且轻松地安装或更新它们。
- 一个打包器，比如webpack或Parcel。它能让你编写模块化代码，并将它们组合在一起成为小的package，以优化加载时间。
- 一个编译器，例如Babel。它能让你编写的新版本JavaScript代码，在旧版浏览器中依然能够工作。

##### JSX简介

JSX，是一个JavaScript的语法扩展。建议在React中配合使用JSX，JSX可以很好地描述UI应该呈现出它应有交互的本质形式。JSX可能会使人联想到模板语言，但它具有JavaScript的全部功能。

###### 为什么使用JSX？

React认为渲染逻辑本质上与其他逻辑内在耦合，比如，在UI中需要绑定事件处理、在某些时刻状态发生变化时需要通知到UI，以及需要在UI中展示准备好的数据。

React并没有采用将标记与逻辑进行分离到不同文件这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现关注点分离。

React不强制要求使用JSX，但是大多数人发现，在JavaScript代码中将JSX和UI放在一起时，会在视觉上有辅助作用。它还可以使React显示更多有用的错误和警告消息。

在JSX语法中，你可以在大括号内放置任何有效的JavaScript表达式。

###### JSX防止注入攻击 

你可以安全地在JSX当中插入用户输入内容。React DOM在渲染所有输入内容分之前，默认会进行转义。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止XSS(cross-site-scripting，跨站脚本)攻击。

###### JSX表示对象 

Babel会把JSX转译成一个名为React.createElement()函数调用。

##### 元素渲染

元素是构成React应用的最小砖块。元素描述了你在屏幕上想看到的内容。

与浏览器的DOM元素不同，React元素是创建开销极小的普通对象。React DOM会负责更新DOM来与React元素保持一致。

###### 注意：组件是由元素构成的。

###### 更新已渲染的元素

React元素是不可变对象。一旦被创建，你就无法更改它的子元素或者属性。

根据我们已有的知识，更新UI唯一的方式是创建一个全新的元素，并将其传入ReactDOM.render()。

###### React只更新它需要更新的部分

React DOM会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使DOM达到预期的状态。

##### 组件&Props

组件允许你将UI拆分为独立可复用的代码片段，并对每个片段进行独立思考。

组件，从概念上类似于JavaScript函数。它接受任意的入参（即“props”），并返回用于描述页面展示内容的React元素。

###### 函数组件与class组件

定义组件最简单的方式就是编写JavaScript函数。

```
function Welcome(props){
	return <h1>Hello,{props.name}</h1>;
}
```

该函数式一个有效的React组件，因为它接收唯一带有数据的props对象与并返回一个React元素。这类组件被称为“函数组件”，因为它本质上就是JavaScript函数。

同时还可以使用ES6的class来定义组件。

```
class Welcome extends React.Component{
    render(){
    	return <h1>Hello,{this.props.name}</h1>
    }
}
```

###### 渲染组件

之前，遇到的React元素都只是DOM标签：

```
const element = <div/>
```

不过，React元素也可以是用户自定义的组件

```
const element = <Welcome name='Sara'>
```

当React元素为用户自定义组件时，它会将JSX所接收的属性（attributes）转换为单个对象传递给组件，这个对象被称之为“props”。

自定义组件名称必须以大写字母开头。React会将以小写字母开头的组件视为原生DOM标签。

###### Props的只读性

组件无论是使用函数声明还是通过class声明，都决不能修改自身的props。

```
function sum(a, b){
	return a + b;
}
```

这样的函数被称为“纯函数”，因为该函数不会尝试更改入参，且多次调用下相同的入参始终返回相同的结果。

相反，下面这个函数则不是纯函数，因为它更改了自己的入参：

```
function withdraw(account, amount){
	account.total -= amount;
}
```

React非常灵活，但它也有一个严格的规则：所有React组件都必须像纯函数一样保护它们的props不被更改。

##### State & 生命周期

State与props类似，但是state是私有的，并且完全受控于当前组件。

现在Clock组件被定义为class，而不是函数。每次组件更新时render方法都会被调用，但只要在相同的DOM节点中渲染<Clock/>，就仅有一个Clock组件的class实例被创建使用。这就使得我们可以使用如state或生命周期方法等很多其他特性。

###### 将生命周期方法添加到Class中

可以为class组件声明一些特殊的方法，当组件挂载或卸载时就会去执行这些方法。这些方法叫做“生命周期方法”。

###### 正确地使用State

1. 不要直接修改State  this.state.comment = 'Hello'; //wrong 而是应该使用setState this.setState({comment: 'hello'})   **构造函数是唯一可以给this.state赋值的地方

2. State的更新可能是异步的

   出于性能考虑，React可能会把多个setState()调用合并成一个调用。

   要解决这个问题，可以让setState()接收一个函数而不是一个对象。这个函数用上一个state作为第一个参数，将此次更新被应用时的props作为第二个参数：

   ```
   this.setState((state, props)=>({
   	counter:state.counter + props.increment
   }));
   ```

​    3.State的更新会被合并

​       当你调用setState()的时候，React会把你提供的对象合并到当前的state。

###### 数据是向下流动的

不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是class组件。这就是为什么称state为局部或是封装的原因。除了拥有并设置了它的组件，其他组件都无法访问。

组件可以选择把它的state作为props向下传递到它的子组件中。这对于自定义组件同样适用。

这通常会被叫做“自上而下”或是“单向”的数据流。任何的state总是所属于特定的组件，而且从该state派生的任何数据或UI只能影响树中“低于”它们的组件。

在React应用中，组件是有状态组价还是无状态组件属于组件实现的细节，它可能会随着时间的推移而改变。你可以在有状态的组件中使用无状态的组件，反之亦然。

Props和State之间最重要的区别是：props由父组件传入，而state由组件本身管理。组件不能修改props，但它可以修改state。

##### 事件处理

React元素的事件处理和DOM元素的很相似，但是有一点语法上的不同：

- React事件的命名采用小驼峰式，而不是纯小写
- 使用JSX语法时你需要传入一个函数作为事件处理函数，而不是一个字符
- 在React中另一个不同点是你不能通过返回false的方式阻止默认行为。你必须显示的使用preventDefault。

 例如，传统的HTML:

```
<button onclick='activateLasers()'>
	Active Lasers
</button>
```

在React中略微不同：

```
<button onClick={activateLasers}>
    Activate Lasers
</button>

```

传统的HTML中阻止链接默认打开一个新页面，可以这样写：

```
<a href='#' onclick="console.log('The link was clicked.'); return flase"></a>

```

在React中，可能是这样的：

```
function ActionLink(){
	funciton handleClick(e){
		e.preventDefault();
		consoel.log('The link was clicked.');
	}
	return (
		<a href='#' onClick={}handleClick>Click me</a>
	)
}

```

在这里，e是一个合成事件。React根据W3C规范来定义这些合成事件，所以你不需要担心跨刘安琪的兼容性问题。

当你使用ES6 class语法定义一个组件的时候，通常的做法是将事件处理函数声明为 class中的方法。

你必须谨慎对待JSX回调函数中的this，在JavaScript中，class的方法默认不会绑定this。如果你忘记绑定this.handleClick并把它传入onClick，当你调用这个函数的时候this的值为undefined。

这并不是React特有的行为；这其实与JavaScript函数工作原理有关。通常情况下，如果你没有在方法后面添加()，例如onClick={this.handleClick}，你应该为这个方法绑定this。

如果觉得使用bind很麻烦，这里有两种方式可以解决。如果你正在使用实验性的public class fields语法，你可以使用class fields 正确的绑定回调函数：

```
class LogginButton extends React.Component{
	//此语法确保`handleCick`内的`this`已被绑定
	//注意：这是*实验性*语法
	handleClick = () =>{
		ocnsole.log('this is :',this)
	}
	render(){
		return(
			<button onClick={this.handleClick}>Click me</button>
		)
	}
}


```

如果你没有使用class fields语法，你可以在回调中使用箭头函数。

```
class LogginButton extends React.Component{
	handleClick(){
		console.log('this is:', this);
	}
	render(){
		return(
			<button onClick={(e)=>this.handelClick(e)}>Click me</button>
		)
	}
}

```

此语法问题在于每次渲染LogginButton时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为prop传入子组件时，这些组件肯会进行额外的重新渲染。我们通常建议在构造器中绑定或使用class fields语法来避免这类性能问题。

###### 向事件处理程序传递参数

在循环中，通常我们会为事件处理函数传递额外的参数。例如，若id是你要删除那一行的ID，以下两种方式都可以向事件处理函数传递参数：

```
<button onClick={(e)=> this.deleteRow(id,e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this,id)}>Delete Row</button>

```

上述两种方式是等价的，分别通过箭头函数和Function.prototype.bind来实现。

在这两种情况下，React的事件对象e会被作为第二个参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过bind的方式，事件对象以及更多的参数将会被隐式的进行传递。

##### 条件渲染

React中的条件渲染和JavaScript中的一样，使用JavaScript运算符if或者条件运算符去创建元素来表现当前的状态，然后让React根据它们来更新UI。

声明一个变量并使用if语句进行条件渲染是不错的方式，但有时你可能会想使用更为简洁的语法。

###### 介绍几种在JSX中内联条件渲染的方法

与运算符&&

通过花括号包裹代码，你可以在JSX中嵌入任何表达式。这也包括JavaScript中的逻辑与(&&)运算符。它可以很方便地进行元素的条件渲染。

之所以能这样做，是因为在JavaScrip中，true && expression 总是返回expression，而false && expression 总是返回false。

因此，如果条件是true，&&右侧的元素就会被渲染，如果是false,React会忽略并跳过它。

三目运算符

另一种内联条件渲染的方法是使用JavaScript中的三目运算符 condition ? true : false。

###### 阻止组件渲染

在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让render方法直接返回null，而不进行任何渲染。

在组件的render方法中返回null并不会影响组件的生命周期。例如，上面这个示例中，componentDidUpdate依然会被调用。

##### 列表& Key

###### 渲染多个组件

可以通过使用{}在JSX内构建一个元素集合。

使用JavaScript中的map()方法来遍历numbers数组。

###### Key

Key帮助React识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。一个元素的key最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用来自数据id来作为元素的key。

###### 用key提取组件

元素的key只有放在就近的数组上下文中才有意义。比方说，如果你提取出一个ListItem组件，你罂告把key保留在数组中的这个<ListItem />元素上，而不是放在ListItem组件中的<li>元素上。

###### key只是在兄弟节点之间必须唯一

数组元素中使用的key在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。当我们生成两个不同的数组时，我们可以使用相同的key值。

##### 表单

###### 受控组件

在HTML中，表单元素（如<input>、<textarea>和<select>）之类的表单元素通常自己维护state，并根据用户输入进行更新。而在React中，可变状态（mutable state）通常保存在组件的state属性中，并且只能通过使用setState()来更新。

我们可以把两者结合起来，使React的state成为“唯一数据源”。渲染表单的React组件还控制着用户输入过程中表单发生的操作。被React以这种方式控制取值的表单输入元素就叫做“受控组件”。

##### 状态提升

通常，多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去。

在React中，将多个组件中需要共享的state向上移动到它们的最近共同父组件中，便可实现共享state。这就是所谓的“状态提升”。

在React应用中，任何可变数据应当只有一个相对应的唯一“数据源”。通常，state都是首先添加到需要渲染数据的组件中去。然后，如果其他组件也需要这个state，那么你可以将它提升至这些组件的最近共同父组件中。你应当依靠自上而下的数据流，而不是尝试在不同组件间同步state。

##### 组合 vs 继承

React有十分强大的组合模式。我们推荐使用组合而非继承来实现组件间的代码重用。

###### 那么继承呢？

在Facebook，我们在成百上千个组件中使用React。我们并没有发现需要使用继承来构建组件层次的情况。

Props和组合为你提供了清晰而安全地定制组件外观和行为的灵活方式。注意：组件可以接受任意props，包括基本数据类型，React元素以及函数。

如果你想要在组件间复用非UI的功能，我们建议将其提取为一个单独的JavaScript模块，如函数、对象或者类。组件可以直接引入（import）而无需通过extend继承它们。

##### React哲学

根据单一功能原则来判定组件的范围。也就是说，一个组件原则上只能负责一个功能。

##### 代码分隔

###### 打包

大多数React应用都会使用Webpack或Browserify这类的构建工具来打包文件。打包是一个将文件引入并合并到一个单独文件的过程，最终形成一个“bundle”。接着在页面上引入该bundle，整个应用即可一次性加载。

###### 代码分隔

打包是个非常棒的技术，但随着你的应用增长，你的代码包也将随之增长。尤其是在整合了体积巨大的第三方库的情况下。你需要关注你代码包中所包含的代码，以避免因体积过大而导致加载时间过长。

为了避免搞出大体积的代码包，在前期就思考该问题并对代码包进行分割是个不错的选择。代码分割是由诸如Webpack(代码分割)和Browserify(factor-bundle)这类打包器支持的一项技术，能够创建多个包并在运行时动态加载。

对你的应用进行代码分割能够帮助你“懒加载”当前用户所需要的内容，能够显著地提高你的应用性能。尽管并没有减少应用整体的代码体积，但你可以避免加载用户永远不需要的代码，并在初始加载的时候减少所需加载的代码量。

###### 基于路由的代码分割

##### Context

Context提供了一个无需为每层组件手动添加props，就能在组件树间进行数据传递的方法。

Context主要应用场景在于很多不同层级的组件需要访问同样一些的数据。请谨慎使用，因为这会使得组件的复用性变差。

如果你只是想避免层层传递一些属性，组件组合（component composition）有时候是一个比context更好的解决方案。

使用context的通用的场景包括管理当前的locale，theme，或者一些缓存数据，这比替代方案要简单的多。

##### 错误边界

过去，组件内的JavaScript错误会导致React的内部状态被破坏，并且在下一次渲染时产生可能无法追踪的错误。这些错误基本上是由较早的其他代码（非React组件代码）错误引起的，但React并没有提供一种在组件中优雅处理这些错误的方式，也无法从错误中恢复。

###### 错误边界（Error Boundaries）

部分UI的JavaScript错误不应该导致整个应用崩溃，为了解决这个问题，React16引入了一个新的概念----错误边界。

错误边界是一种React组件，这种组件可以捕获并打印发生在其子组件树任何位置的JavaScript错误，并且，它会渲染出备用UI，而不是渲染那些崩溃了的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

##### Refs转发

Ref转发是一项将ref自动地通过组件传递到其一子组件的技巧。

##### Fragments

React中的一个常见模式是一个组件返回多个元素。Fragments允许你将子列表分组，而无需向DOM添加额外节点。

```
render(){
	return(
		<React.Fragment>
			<ChildA />
			<ChildB />
			<ChildC />
		</React.Fragment>
	)
}

```

使用<></>代替<React.Fragment></React.Fragment>，不支持key或属性。

key是唯一可以传递给Fragment的属性。

##### 高阶组件

高阶组件(HOC)是React中用于复用组件逻辑的一种高级技巧。HOC自身不是React API的一部分，它是一种基于React的组合特性而形成的设计模式。具体而言，高阶组件是参数为组件，返回值为新组件的函数。

组件是将props转换为UI，而高阶组件是将组件转换为另一个组件。

请注意。HOC不会修改传入的组件，也不会使用继承来复制其行为。相反，HOC通过将组件包装在容器组件中来组成新组件。HOC是纯函数，没有副作用。

###### 不要改变原始组件。使用组合。

不要试图咋HOC中修改组价原型（或以其他方式改变它）。

HOC不应该修改传入组件，而应该使用组合的方式，通过将组件包装在容器组件中实现功能。

注意事项：不要在render方法中使用HOC、务必复制静态方法、Refs不会被传递

##### 深入JSX

实际上，JSX仅仅只是React.createElement(component, props, ...children)函数的语法糖。

if语句以及for循环不是JavaScript表达式，所以不能在JSX中直接使用。但是，你可以用在JSX以外的代码中。

###### JSX中的子元素

包含在开始和结束标签之间的JSX表达式内容将作为特定属性props.children传递给外层组件。有几种不同的方法来传递子元素

###### 布尔类型、Null以及Undefined将会忽略

false，null，undefined，true是合法的子元素。但它们并不会被渲染。

##### 性能优化

在开发应用时使用开发模式，而在为用户部署时使用生产模式。

如果你浏览一个基于React开发模式的网站，图标背景色会变成红色。如果你浏览一个基于React生产版本的网站，图标背景色会变成深色。

##### Portals

Portal提供了一种将子节点渲染到存在于父组件以外的DOM节点的优秀的方案。

```
ReactDOM.createPortal(child, container)

```

第一个参数（child）是任何可渲染的React子元素，例如一个元素，字符串或fragment。第二个参数（container）是一个DOM元素。

通常来讲，当你从组件的render方法返回一个元素时，该元素将被挂载到DOM节点中离其最近的父节点。

##### 不使用ES6

###### 通常会使用JavaScript的class关键字来定义React组件。

###### 声明默认属性

无论是函数组件还是class组件，都拥有defaultProps属性

###### 初始化State

如果使用ES6的class关键字创建组件，可以通过给this.state赋值的方式来定义组件的初始state。

###### 自动绑定

对于使用ES6的class关键字创建的React组件，组件中的方法遵循与常规ES6 class相同的语法规则。这意味着这些方法不会自动绑定this到这个组件实例。你需要在constructor中显式地调用.bind(this)。

ES6本身不包含任何mixin支持。因此，当你在React中使用ES6 class时，将不支持mixins。

##### 生命周期方法

###### React16以前的生命周期方法：

挂载：constructor()、componentWillMount()、render()、componentDidMount()

更新：props change ->componentWillReceiveProps 、 state change->shouldComponentUpdate 、componentWillUdate、render()、componentDidUpdate()

卸载：componentWillUnmount()

###### React16以后的生命周期方法：（16可以用，17是 正式的）

挂载：当组件实例被创建并插入DOM中时，其生命周期调用顺序如下：

constructor(props)、static getDerivedStateFromProps()、render()、componentDidMount()

注意：下述生命周期方法即将过时，在新代码中应该避免使用它们：

UNSAFE_componentWillMount()

更新：当组件的props或state发生变化时会触发更新。组件更新的生命周期调用顺序如下：

static getDerivedStateFromProps(props, state)、

shouldComponentUpdate(nextPorps, nextState)、render()、getSnapshotBeforeUpdate()、componentDidUpdate(prevProps, prevState, snapshot)

注意：下述方法即将过时，在新代码中应该避免使用它们：

UNSAFE_componentWillUpdate()、UNSAFE_componentWillReceiveProps()

卸载：当组件从DOM中移除时会调用如下方法：

componentWillUnmount()

错误处理：当渲染过程中，生命周期或子组件的构造函数中抛出错误时，会调用如下方法：

static getDerivedStateFromError()、componentDidCatch()

###### 常用的生命周期方法

```
render() 是class组件中唯一必须实现的方法。

```

```
constructor(props)如果不初始化state或不进行方法绑定，则不需要为React组件实现构造函数。

```

在React组件挂载之前，会调用它的构造函数。在为React.Component子类实现构造函数时，应在其他语句之前调用super(props)。否则，this.props在构造函数中可能会出现未定义的bug。

通常，在React中，构造函数仅用于以下两种情况：

- 通过给this.state赋值对象来初始化内部state。
- 为事件处理函数绑定实例。

在constructor()函数中不要调用setState()方法。如果你的组件需要使用内部state,请直接在构造函数中为this.state赋值初始state。

避免将props的值复制给state!这是一个常见的错误

```
componentDidMount() 会在组件挂载后（插入DOM树中）立即调用。依赖于DOM节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。

```

```
componentDidUpdate(prevProps, prevState, snapshot) 会在更新后会被立即调用。首次渲染不会执行此方法。

```

当组件更新后，可以在此处对DOM进行操作。如果你对更新前后的props进行了比较，也可以选择在此处进行网络请求。（例如，当props未发生变化时，则不会执行网络请求）。

如果组件实现了getSnapshotBeforeUpdate()生命周期，则它的返回值将作为componentDidUpdate()的第三个参数‘snapshot’参数传递，否则此参数将为undefined。

```
componentWillUnmount()会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除timer，取消网络请求或清除在componentDidMount()中创建的订阅等。

```

componentWillUnmount()中不应调用setState()，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。

###### 不常用的生命周期方法

```
shouldComponentUpdate(nextProps, nextState)

```

根据shouldComponentUpdate(nextprops, nextState)的返回值，判断React组件的输出是否受当前state或props更改的影响。默认行为是state每次发生变化组件都会重新渲染。大部分情况下，你应该遵循默认行为。

当props或state发生变化时，shouldComponentUpdate()会在熏染执行之前被调用。返回值默认为true。首次渲染或使用forceUpdate()时不会调用该方法。

目前，如果shouldComponentUpdate()返回false，则不会调用UNSAFE_componentWillUpdate，render()和componentDidUpdate()。后续版本，React可能会将shouldComponentUpdate视为提示而不是严格的指令，并且，当返回false时，仍可能导致组件重新渲染。

```
static getDerivedStateFromProps(props,state)

```

getDerivedStateFromProps会在调用render方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新state，如果返回null则不更新任何内容。

此方法适用于罕见的用例，即state的值在任何时候都取决于props。例如，实现<Transition>组件可能很方便，该组件会比较当前组件与下一组件，以决定针对哪些组件进行转场动画。

请注意，不管原因是什么，都会在每次渲染前触发此方法。这与UNSAFE_componentWillReceiveProps形成对比，后者仅在父组件重新渲染时触发，而不是在内部调用setState时。

```
getSnapshotBeforeUpdate(prevProps, prevState)

```

getSnapshotBeforeUpdate()在最近一次渲染输出（提交到DOM节点）之前调用。它使得组件能在发生更改之前从DOM中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给componentDidUpdate()。

此用法并不常见，但它可能出现在UI处理中，如需要以特殊方式处理滚动位置的聊天线程等。

应返回snapshot的值（或null）。

##### Error boundaries

Error boundaries 是React组件，它会在其子组件树中的任何位置捕获JavaScript错误，并记录这些错误，展示降级UI而不是崩溃的组件树。Error boundaries组件会捕获在渲染期间，在生命周期方法以及其整个树的构造函数中发生的错误。

如果class组件定义了生命周期方法 static getDerivedStateFromError()或componentDidCatch()中的任何一个（或两个），它就成为了Error boundaries()。通过生命周期更新state可让组件捕获树中未处理的JavaScript错误并展示降级UI。

仅适用Error boundaries组件来从意外异常中恢复的情况；不要将它们用于流程控制。

Error boundaries 仅捕获组件树中以下组件中的错误。但它本身的错误无法捕获。

```
static getDerivedStateFromError(error)

```

此生命周期会在后代组件抛出错误后被调用。它将抛出的错误作为参数，并返回一个值以更新state。

getDerivedStateFromError()h会在渲染阶段调用，因此不允许出现副作用。如遇此类情况，请改用componentDidCatch()。

```
componentDidCatch(error, info)此生命周期在后代组件抛出错误后被调用

```

接收2个参数： error - 抛出的错误  info-带有componentStack key的对象，其中包含有关组件引发错误的栈信息

componentDidCatch()会在‘提交’阶段被调用，因此允许执行副作用。它应该用于记录错误之类的情况。

##### 合成事件

剪贴板事件

```
onCopy onCut onPaste

```

复合事件

```
onCompositionEnd onCompositionStart onCompositionUpdate

```

键盘事件

```
onKeyDown onKeyPress onKeyUp

```

焦点事件

```
onFocus onBlur

```

表单事件

```
onChange onInput onInvalid onSubmit

```

鼠标事件

```
onClick onContextMenu onDoubleClick onDrag onDragEnd onDragEnter onDragExit
onDragLeave onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave onMouseMove onMouseOut onMouseOver onMouseUp

```

指针事件

```
onPointerDown onPointerMove onPointerUp onPointerCancel onGotPointerCapture 
onLostPointerCapture onPointerEnter onPointerLeave onPointerOver onPointerOut

```

选择事件

```
onSelect

```

触摸事件

```
onTouchCancel onTouchEnd onTouchMove onTouchStart

```

UI事件

```
onScroll

```

滚轮事件

```
onWheel

```

媒体事件

```
onAbort onCanPlay onCanPlayThrough onDurationChange onEmptied onEncrypted onEnded onError onLoadedData onLoadedMetadata onLoadStart onPause onPlay onPlaying onProgress onRateChange onSeeked onSeeking onStalled onSuspend onTimeUpdate onVolumeChange onWaiting 

```

图像事件

```
onLoad onError

```

动画事件

```
onAnimationStart onAnimationEnd onAnimationIteration

```

过渡事件

```
onTransitionEnd

```

其他事件

```
onToggle

```

##### 单页面应用

单页面应用（single-page application），是一个应用程序，它可以加载单个HTML页面，以及运行应用程序所需的所有必要资源（例如JavaScript 和 CSS）。与页面或后续页面的任何交互，都不再需要往返server加载资源，即页面不会重新加载。

##### 受控组件 vs 非受控组件

React有两种不同的方式来处理表单输入。

如果有一个input表单元素的值是由React控制，就其称为受控组件。当用户将数据输入到受控组件时，会触发修改状态的事件处理器，这时由你的代码来决定此输入是否有效（如果有效就使用更新后的值重新渲染）。入股不重新渲染，则表单元素将保持不变。

一个非受控组件，就像是运行在React体系之外的表单元素。当用户将数据输入到表单字段（例如input,dropdown等）时，React不需要做任何事情就可以映射更新后的信息。然而，这也意味着，你无法强制给这个表单字段设置一个特定值。

在大多数情况下，你应该使用受控组件。

##### 协调

当组件的pros或state发生变化时，React通过将最新返回的元素与原先渲染的元素进行比较，来决定是否有必要进行一次实际的DOM更新。当它们不相等时，React才会更新DOM。这个过程被称为“协调”。

setState什么时候是异步的？目前，在事件处理函数内部的setState是异步的。
