#### React笔记

###### React是什么？

React是一个声明式，高效且灵活的用于构建用户界面的JavaScript裤。使用React可以将一些简短、独立的代码片段组合成复杂的UI界面，这些代码片段被称作“组件”。

React中拥有多种不同类型的组件。

一个组件接收一些参数，我们把这些参数叫做props('props'是‘properties’简写)，然后通过render方法返回需要展示在屏幕上的视图的层次结构。

render方法的返回值描述了你希望在屏幕上看到的内容。React根据描述，然后把结果展示出来。更具体地说，render返回了一个React元素，这是一种对渲染内容的轻量级描述。

在JSX中你可以任意使用JavaScript表达式，只需要用一个大括号把表达式括起来。每一个React元素事实上都是一个JavaScript对象，你可以在你的程序中把它当保存在变量中或者作为参数传递。

在React应用中，数据通过props的传递，从父组件流向子组件。

```
class Square extends React.Component {
	render(){
		<button className='square' onClick={()=>alert('click')>
			{this.props.value}
		</button>
	}
}
```

注意，此处使用了 onClic={()=>alert('click')}的方式向onClick这个prop传入一个函数。React将在单击时调用此函数。很多人经常忘记编写()=>,而写成了onClick={alert('click')}，这种常见的错误会导致每次这个组件渲染的时候都会触发弹出框。

可以通过在React组件的构造函数中设置this.state来初始化state。this.state应该被视为一个组件的私有属性。

在JavaScript中，每次你定义其子类的构造函数时，都需要调用super方法。因此，在所有含有构造函数的React组件中，构造函数必须以super(props)开头。

每次在组件中调用setState时，React都会自动更新其子组件。

**当你遇到需要同时获取多个子组件数据，或者两个组件之间需要相互通讯的情况时，需要把子组件的 state 数据提升至其共同的父组件当中保存。之后父组件可以通过 props 将状态数据传递到子组件当中。这样应用当中所有组件的状态数据就能够更方便地同步共享了。**

###### 为什么不可变性在 React 中非常重要

一般来说，有两种改变数据的方式。第一种方式是直接*修改*变量的值，第二种方式是使用新的一份数据替换旧数据。

直接修改数据

```
var player = {score: 1, name: 'Jeff'};
player.score = 2;
//player修改后的值为 {score: 2, name: 'Jeff'}
```

新数据替换旧数据

```
var player = {score: 1, name: 'Jeff'};
var newPlayer = Object.assign({},player,{score:2});
//player的值没有改变，但是newPlayer的值是{score: 2, name: 'Jeff'}
//使用对象展开语法，就可以写成：
var newPlayer = {...player,score:2};
```

不直接修改（或改变底层数据）这种方式和前一种方式的结果是一样的，这种方式有以下几点好处：

###### 简化复杂的功能

不可变性使得复杂的特性更容易实现。在后面的章节里，我们会实现一种叫做“时间旅行”的功能。“时间旅行”可以使我们回顾井字棋的历史步骤，并且可以“跳回”之前的步骤。这个功能并不是只有游戏才会用到——撤销和恢复功能在开发中是一个很常见的需求。不直接在数据上修改可以让我们追溯并复用游戏的历史记录。

###### 跟踪数据的改变

如果直接修改数据，那么就很难跟踪到数据的改变。跟踪数据的改变需要可变对象可以与改变之前的版本进行对比，这样整个对象树都需要被遍历一次。

跟踪不可变数据的变化相对来说就容易多了。如果发现对象变成了一个新对象，那么我们就可以说对象发生改变了。

###### 确定在 React 中何时重新渲染

不可变性最主要的优势在于它可以帮助我们在 React 中创建 *pure components*。我们可以很轻松的确定不可变数据是否发生了改变，从而确定何时对组件进行重新渲染。

###### 函数组件

如果你想写的组件只包含一个 `render` 方法，并且不包含 state，那么使用**函数组件**就会更简单。我们不需要定义一个继承于 `React.Component` 的类，我们可以定义一个函数，这个函数接收 `props` 作为参数，然后返回需要渲染的元素。函数组件写起来并不像 class 组件那么繁琐，很多组件都可以使用函数组件来写。

```
function Square(props){
	return(
		<button className="square" onClick={props.onClick}>
			{props.value}
		</button>
	)
}
```

`concat()` 方法可能与你比较熟悉的 `push()` 方法不太一样，它并不会改变原数组，所以我们推荐使用 `concat()`。

**我们强烈推荐，每次只要你构建动态列表的时候，都要指定一个合适的 key。**

组件的 key 值并不需要在全局都保证唯一，只需要在当前的同一级元素之前保证唯一即可。

###### 一组JavaScript构建工具链通常由这些组成：

- 一个package管理器，比如Yarn或npm 。它能让你充分利用庞大的第三方package的生态系统，并且轻松地安装或更新它们。
- 一个打包器，比如webpack或Parcel。它能让你编写模块化代码，并将它们组合在一个成为小的package，以优化加载时间。
- 一个编译器，例如Babel。它能让你编写的新版本JavaScript代码，在旧版本浏览器中依然能够工作。

###### JSX简介

```
const element = <h1>Hello,world!</h1>;
```

这个有趣的标签语法既不是字符串也不是 HTML。

它被称为 JSX，是一个 JavaScript 的语法扩展。我们建议在 React 中配合使用 JSX，JSX 可以很好地描述 UI 应该呈现出它应有交互的本质形式。JSX 可能会使人联想到模版语言，但它具有 JavaScript 的全部功能。

JSX 可以生成 React “元素”。

###### 为什么使用JSX?

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，比如，在 UI 中需要绑定处理事件、在某些时刻状态发生变化时需要通知到 UI，以及需要在 UI 中展示准备好的数据。

React 并没有采用将*标记与逻辑进行分离到不同文件*这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现[*关注点分离*](https://en.wikipedia.org/wiki/Separation_of_concerns)。

React [不强制要求](https://reactjs.bootcss.com/docs/react-without-jsx.html)使用 JSX，但是大多数人发现，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。它还可以使 React 显示更多有用的错误和警告消息。

###### 在JSX中嵌入表达式

在 JSX 语法中，你可以在大括号内放置任何有效的 [JavaScript 表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)。例如，`2 + 2`，`user.firstName` 或 `formatName(user)` 都是有效的 JavaScript 表达式。

为了便于阅读，我们会将 JSX 拆分为多行。同时，我们建议将内容包裹在括号中，虽然这样做不是强制要求的，但是这可以避免遇到[自动插入分号](http://stackoverflow.com/q/2846283)陷阱。

###### JSX也是一个表达式

在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

也就是说，你可以在 `if` 语句和 `for` 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：

```
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

###### JSX特定属性

你可以通过使用引号，来将属性值指定为字符串字面量：

```
const element = <div tabIndex="0"></div>;
```

也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：

```
const element = <img src={user.avatarUrl}></img>;
```

在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。你应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，对于同一属性不能同时使用这两种符号。

警告：因为JSX语法上更接近JavaScript而不是HTML，所以React DOM使用camelCase(小驼峰命名)来定义属性的名称，而不使用HTML属性名称的命名约定。

例如，JSX里的class变成了className，而tabindex则变为tabIndex。

###### 使用JSX指定子元素

假如一个标签里面没有内容，你可以使用 `/>` 来闭合标签，就像 XML 语法一样：

JSX 标签里能够包含很多子元素:

###### JSX防止注入攻击

React DOM 在渲染所有输入内容之前，默认会进行[转义](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html)。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 [XSS（cross-site-scripting, 跨站脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。

###### JSX表示对象

Babel会把JSX转译成一个名为React.createElement()函数调用。

`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码。

###### 元素渲染

元素是构成React应用的最小砖块。元素描述了你在屏幕上想看到的内容。

与浏览器的DOM元素不同，React元素是创建开销极小的普通对象。React DOM会负责更新DOM来与React元素保持一致。

组件是由元素构成的。

###### 将一个元素渲染为DOM

仅使用 React 构建的应用通常只有单一的根 DOM 节点。

想要将一个 React 元素渲染到根 DOM 节点中，只需把它们一起传入 [`ReactDOM.render()`](https://reactjs.bootcss.com/docs/react-dom.html#render)

###### 更新已渲染的元素

React 元素是[不可变对象](https://en.wikipedia.org/wiki/Immutable_object)。一旦被创建，你就无法更改它的子元素或者属性。一个元素就像电影的单帧：它代表了某个特定时刻的 UI。

根据我们已有的知识，更新 UI 唯一的方式是创建一个全新的元素，并将其传入 [`ReactDOM.render()`](https://reactjs.bootcss.com/docs/react-dom.html#render)。

###### React只更新它需要更新的部分

React DOM会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使DOM达到预期的状态。

###### 组件&Props

组件允许你将UI拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似于JavaScript函数。它接受任意的入参（即“props”）,并返回用于描述页面展示内容的React元素。

###### 函数组件与class组件

定义组件最简单的方式就是编写JavaScript函数

```
function Welcome(props){
	return <h1>Hello, {props.name}</h1>;
}
```

该函数是一个有效的React组件，因为它接收唯一带有数据的“props”(代表属性)对象与并返回一个React元素。这类组件被称为“函数组件”，因为它本质上就是JavaScript函数。

同时还可以使用ES6的class来定义组件：

```
class Welcome extends React.Component{
	render(){
		return <h1>Hello, {this.props.name}</h1>;
	}
}
```

上述两个组件在React里是等效的。

###### 渲染组件

之前，我们遇到的React元素都只是DOM标签：

```
const element = <div />
```

不过，React元素也可以是用户自定义的组件：

```
const element = <Welcome name='Sara'>;
```

当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）以及子组件（children）转换为单个对象传递给组件，这个对象被称之为 “props”。

注意：组件名称必须以大写字母开头。React会将以小写字母开头的组件视为原生DOM标签。

###### 组合组件

组件可以在其输出中引用其他组件。这就可以让我们用同一组件来抽象出任意层次的细节。按钮、表单、对话框、甚至整个屏幕的内容：在React应用程序中，这些通常都会以组件的形式表示。

###### 提取组件

将组件拆分为更小的组件。

###### Props的只读性

组件无论是使用函数声明还是通过class声明，都决不能修改自身的props。

所有React组件都必须像纯函数一样保护它们的props不被更改。

###### State & 生命周期

在元素渲染章节中，我们只了解了一种更新UI界面的方法。通过调用ReactDOM.render()来修改我们想要渲染的元素。

State与props类似，但是state是私有的，并且完全受控于当前组件。

###### 将函数组件转换成class组件

通过以下五步将Clock的函数组件转成class组件：

1. 创建一个同名的ES6 class，并且继承于React.Component。
2. 添加一个空的render()方法。
3. 将函数体移动到render()方法之中。
4. 在render()方法中使用this.props替换props。
5. 删除剩余的空函数声明。

###### 向class组件中添加局部的state

###### 将生命周期方法添加到Class中

在具有许多组件的应用程序中，当组件被销毁时释放所占用的资源是非常重要的。

###### 正确地使用State

###### 不要直接修改State 

而是应该使用setState()

###### State的更新可能是异步的

出于性能考虑，React可能会把多个setState()调用合并成一个调用。

因为this.props和this.state可能会异步更新，所以不要依赖他们的值来更新下一个状态。

###### State的更新会被合并

当你调用setState()的时候，React会把你提供的对象合并到当前的state。

###### 数据是向下流动的

不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是 class 组件。

这就是为什么称 state 为局部的或是封装的的原因。除了拥有并设置了它的组件，其他组件都无法访问。

组件可以选择把它的 state 作为 props 向下传递到它的子组件中。

这通常会被叫做“自上而下”或是“单向”的数据流。任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件。

如果你把一个以组件构成的树想象成一个 props 的数据瀑布的话，那么每一个组件的 state 就像是在任意一点上给瀑布增加额外的水源，但是它只能向下流动。

在 React 应用中，组件是有状态组件还是无状态组件属于组件实现的细节，它可能会随着时间的推移而改变。你可以在有状态的组件中使用无状态的组件，反之亦然。

###### 事件处理

React元素的事件处理和DOM元素的很相似，但是有一点语法上的不同：

- React事件的命名采用小驼峰式（camelCase）,而不是纯小写。
- 使用JSX语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。

例如，传统的HTML:

```
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

在React中略微不同：

```
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

在 React 中另一个不同点是你不能通过返回 `false` 的方式阻止默认行为。你必须显式的使用 `preventDefault` 。例如，传统的 HTML 中阻止链接默认打开一个新页面，你可以这样写：

```
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

在React中，可能是这样的：

```
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

在这里，`e` 是一个合成事件。React 根据 [W3C 规范](https://www.w3.org/TR/DOM-Level-3-Events/)来定义这些合成事件，所以你不需要担心跨浏览器的兼容性问题。

使用 React 时，你一般不需要使用 `addEventListener` 为已创建的 DOM 元素添加监听器。事实上，你只需要在该元素初始渲染的时候添加监听器即可。

当你使用 [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 语法定义一个组件的时候，通常的做法是将事件处理函数声明为 class 中的方法。

你必须谨慎对待 JSX 回调函数中的 `this`，在 JavaScript 中，class 的方法默认不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) `this`。如果你忘记绑定 `this.handleClick` 并把它传入了 `onClick`，当你调用这个函数的时候 `this` 的值为 `undefined`。

这并不是 React 特有的行为；这其实与 [JavaScript 函数工作原理](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)有关。通常情况下，如果你没有在方法后面添加 `()`，例如 `onClick={this.handleClick}`，你应该为这个方法绑定 `this`。

如果觉得使用bind很麻烦，这里有两种方式可以解决。如果你正在使用实验性的public class fields语法，你可以使用class fields正确的绑定回调函数

```
class LogginButton extends React.Component{
	//此语法确保'handleClick'内的this已被绑定
	//注意:这是“实验性”语法
	handleClick= ()=>{
		console.log('this is:',this)
	}
	
	render(){
		return(
			<button onClick={this.handleClick}>
				click name
			</button>	
		)
	}
}
```

如果你没有使用class fields语法，你可以在回调中使用箭头函数：

```
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 此语法确保 `handleClick` 内的 `this` 已被绑定。
    return (
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
}
```

此语法问题在于每次渲染 `LoggingButton` 时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。我们通常建议在构造器中绑定或使用 class fields 语法来避免这类性能问题。

###### 向事件处理程序传递参数

在循环中，通常我们会为事件处理函数传递额外的参数。例如，若id是你要删除那一行的ID，以下两种方式都可以向事件处理函数传递参数

```
<button onClick={(e)=>this.deleteRow(id,e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this,id)}>Delete Row</button>
```

上述两种方式是等价的，分别通过箭头函数和Function.prototype.bind来实现。

在这两种情况下，React的事件对象e会被作为第二个参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过bind的方式，事件对象以及更多的参数将会被隐式的进行传递。

###### 条件渲染

React中的条件渲染和JavaScript中的一样，使用JavaScript运算符if或者条件运算符去创建元素来表现当前的状态，然后让React根据它们来更新UI。

###### 元素变量

你可以使用变量来储存元素。它可以帮助你有条件地渲染组件的一部分，而其他的渲染部分并不会因此而改变。

###### 与运算符&&

通过花括号包裹代码，你可以[在 JSX 中嵌入任何表达式](https://reactjs.bootcss.com/docs/introducing-jsx.html#embedding-expressions-in-jsx)。这也包括 JavaScript 中的逻辑与 (&&) 运算符。它可以很方便地进行元素的条件渲染。

之所以能这样做，是因为在 JavaScript 中，`true && expression` 总是会返回 `expression`, 而 `false && expression` 总是会返回 `false`。

因此，如果条件是 `true`，`&&` 右侧的元素就会被渲染，如果是 `false`，React 会忽略并跳过它。

###### 三目运算符

另一种内联条件渲染的方法是使用 JavaScript 中的三目运算符 [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。

###### 阻止组件渲染

在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让 `render` 方法直接返回 `null`，而不进行任何渲染。

在组件的 `render` 方法中返回 `null` 并不会影响组件的生命周期。例如，上面这个示例中，`componentDidUpdate` 依然会被调用。

##### 列表 & Key

###### 渲染多个组件

可以通过使用{}在JSX内构建一个元素集合。

###### Key

Key帮助React识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用数据中的 id 来作为元素的 key。

###### 用 key 提取组件

元素的key只有放在就近的数组上下文中才有意义。

比方说，如果你[提取](https://reactjs.bootcss.com/docs/components-and-props.html#extracting-components)出一个 `ListItem` 组件，你应该把 key 保留在数组中的这个 `<ListItem />` 元素上，而不是放在 `ListItem` 组件中的 `<li>` 元素上。

###### key 只是在兄弟节点之间必须唯一

数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。

##### 表单

在 React 里，HTML 表单元素的工作方式和其他的 DOM 元素有些不同，这是因为表单元素通常会保持一些内部的 state。

```
<form>
  <label>
    名字:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="提交" />
</form>
```

此表单具有默认的 HTML 表单行为，即在用户提交表单后浏览到新页面。如果你在 React 中执行相同的代码，它依然有效。但大多数情况下，使用 JavaScript 函数可以很方便的处理表单的提交， 同时还可以访问用户填写的表单数据。实现这种效果的标准方式是使用“受控组件”。

###### 受控组件

在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）之类的表单元素通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 [`setState()`](https://reactjs.bootcss.com/docs/react-component.html#setstate)来更新。

我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

对于受控组件来说，输入的值始终由 React 的 state 驱动。你也可以将 value 传递给其他 UI 元素，或者通过其他事件处理函数重置，但这意味着你需要编写更多的代码。

总的来说，这使得 `<input type="text">`, `<textarea>` 和 `<select>` 之类的标签都非常相似—它们都接受一个 `value` 属性，你可以使用它来实现受控组件。

###### 受控输入空值

在[受控组件](https://reactjs.bootcss.com/docs/forms.html#controlled-components)上指定 value 的 prop 会阻止用户更改输入。如果你指定了 `value`，但输入仍可编辑，则可能是你意外地将`value` 设置为 `undefined` 或 `null`。

###### 状态提升

通常，多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去。

在 React 中，将多个组件中需要共享的 state 向上移动到它们的最近共同父组件中，便可实现共享 state。这就是所谓的“状态提升”。

在 React 应用中，任何可变数据应当只有一个相对应的唯一“数据源”。通常，state 都是首先添加到需要渲染数据的组件中去。然后，如果其他组件也需要这个 state，那么你可以将它提升至这些组件的最近共同父组件中。你应当依靠[自上而下的数据流](https://reactjs.bootcss.com/docs/state-and-lifecycle.html#the-data-flows-down)，而不是尝试在不同组件间同步 state。

##### 组合 VS 继承

React 有十分强大的组合模式。我们推荐使用组合而非继承来实现组件间的代码重用。

组合也同样适用于以 class 形式定义的组件。

###### 那么继承呢？

在Facebook，我们在成千上百个组件中使用react。我们并没有发现需要使用继承来构建组件层次的情况。

Props和组合为你提供了清晰而安全地定制组件外观和行为的灵活方式。注意：组件可以接受任意props，包括基本数据类型，React元素以及函数。

如果你想要在组件间复用非UI的功能，建议将其提取为一个单独的JavaScript模块，如函数、对象或者类。组件可以直接引入（import）而无需通过extend继承它们。

##### React哲学

我们认为，React 是用 JavaScript 构建快速响应的大型 Web 应用程序的首选方式。它在 Facebook 和 Instagram 上表现优秀。

React 最棒的部分之一是引导我们思考如何构建一个应用。

##### 无障碍辅助功能

###### 为什么我们需要无障碍辅助功能？

网络无障碍辅助功能（Accessibility，也被称为 [**a11y**](https://en.wiktionary.org/wiki/a11y)，因为以 A 开头，以 Y 结尾，中间一共 11 个字母）是一种可以帮助所有人获得服务的设计和创造。无障碍辅助功能是使得辅助技术正确解读网页的必要条件。

React 对于创建可访问网站有着全面的支持，而这通常是通过标准 HTML 技术实现的。

##### 代码分隔

###### 打包

大多数 React 应用都会使用 [Webpack](https://webpack.docschina.org/)，[Rollup](https://rollupjs.org/) 或 [Browserify](http://browserify.org/) 这类的构建工具来打包文件。 打包是一个将文件引入并合并到一个单独文件的过程，最终形成一个 “bundle”。 接着在页面上引入该 bundle，整个应用即可一次性加载。

###### 代码分隔

打包是个非常棒的技术，但随着你的应用增长，你的代码包也将随之增长。尤其是在整合了体积巨大的第三方库的情况下。你需要关注你代码包中所包含的代码，以避免因体积过大而导致加载时间过长。

为了避免搞出大体积的代码包，在前期就思考该问题并对代码包进行分割是个不错的选择。 代码分割是由诸如 [Webpack](https://webpack.docschina.org/guides/code-splitting/)，[Rollup](https://rollupjs.org/guide/en/#code-splitting) 和 Browserify（[factor-bundle](https://github.com/browserify/factor-bundle)）这类打包器支持的一项技术，能够创建多个包并在运行时动态加载。

对你的应用进行代码分割能够帮助你“懒加载”当前用户所需要的内容，能够显著地提高你的应用性能。尽管并没有减少应用整体的代码体积，但你可以避免加载用户永远不需要的代码，并在初始加载的时候减少所需加载的代码量。

###### import()

在你的应用中引入代码分隔的最佳方式是通过动态import()语法。

###### React.lazy

React.lazy和Suspense技术还不支持服务端渲染。如果你想要在使用服务端渲染的应用中使用，我们推荐Loadable Components 这个库。它是一个很棒的服务端渲染打包指南。

React.lazy函数能让你像渲染常规组件一样动态引入（的组件）。

React.lazy接受一个函数，这个函数需要动态调用import()。它必须返回一个Promise,该Promise需要resolve一个default export的React组件。

然后应在 `Suspense` 组件中渲染 lazy 组件，如此使得我们可以使用在等待加载 lazy 组件时做优雅降级（如 loading 指示器等）。

###### 异常捕获边界（Error boundaries）

如果模块加载失败（如网络问题），它会触发一个错误。你可以通过异常捕获边界（Error boundaries）技术来处理这些情况，以显示良好的用户体验并管理恢复事宜。

###### 基于路由的代码分割

决定在哪引入代码分割需要一些技巧。你需要确保选择的位置能够均匀地分割代码包而不会影响用户体验。

一个不错的选择是从路由开始。大多数网络用户习惯于页面之间能有个加载切换过程。你也可以选择重新渲染整个页面，这样您的用户就不必在渲染的同时再和页面上的其他元素进行交互。

###### 命名导出（Named Exports）

`React.lazy` 目前只支持默认导出（default exports）。如果你想被引入的模块使用命名导出（named exports），你可以创建一个中间模块，来重新导出为默认模块。这能保证 tree shaking 不会出错，并且不必引入不需要的组件。

###### Context

Context提供了一个无需为每层组件手动添加props，就能在组件树间进行数据传递的方法。

在一个典型的 React 应用中，数据是通过 props 属性自上而下（由父及子）进行传递的，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，UI 主题），这些属性是应用程序中许多组件都需要的。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。

###### 何时使用 Context

Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。

###### 使用 Context 之前的考虑

Context 主要应用场景在于*很多*不同层级的组件需要访问同样一些的数据。请谨慎使用，因为这会使得组件的复用性变差。

**如果你只是想避免层层传递一些属性，[组件组合（component composition）](https://reactjs.bootcss.com/docs/composition-vs-inheritance.html)有时候是一个比 context 更好的解决方案。**

##### 错误边界

过去，组件内的 JavaScript 错误会导致 React 的内部状态被破坏，并且在下一次渲染时 [产生](https://github.com/facebook/react/issues/4026) [可能无法追踪的](https://github.com/facebook/react/issues/6895) [错误](https://github.com/facebook/react/issues/8579)。这些错误基本上是由较早的其他代码（非 React 组件代码）错误引起的，但 React 并没有提供一种在组件中优雅处理这些错误的方式，也无法从错误中恢复。

###### 错误边界（Error Boundaries）

部分 UI 的 JavaScript 错误不应该导致整个应用崩溃，为了解决这个问题，React 16 引入了一个新的概念 —— 错误边界。

错误边界是一种 React 组件，这种组件**可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会渲染出备用 UI**，而不是渲染那些崩溃了的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

注意

错误边界**无法**捕获以下场景中产生的错误：

- 事件处理（[了解更多](https://reactjs.bootcss.com/docs/error-boundaries.html#how-about-event-handlers)）
- 异步代码（例如 `setTimeout` 或 `requestAnimationFrame` 回调函数）
- 服务端渲染
- 它自身抛出来的错误（并非它的子组件）

###### 错误边界应该放置在哪？

错误边界的粒度由你来决定，可以将其包装在最顶层的路由组件并为用户展示一个 “Something went wrong” 的错误信息，就像服务端框架经常处理崩溃一样。你也可以将单独的部件包装在错误边界以保护应用其他部分不崩溃。

###### 未捕获错误（Uncaught Errors）的新行为

这一改变具有重要意义，**自 React 16 起，任何未被错误边界捕获的错误将会导致整个 React 组件树被卸载。**

###### 关于事件处理器

错误边界**无法**捕获事件处理器内部的错误。

React 不需要错误边界来捕获事件处理器中的错误。与 render 方法和生命周期方法不同，事件处理器不会在渲染期间触发。因此，如果它们抛出异常，React 仍然能够知道需要在屏幕上显示什么。

如果你需要在事件处理器内部捕获错误，使用普通的 JavaScript `try` / `catch` 语句：

##### Refs 转发

Ref 转发是一项将 [ref](https://reactjs.bootcss.com/docs/refs-and-the-dom.html) 自动地通过组件传递到其一子组件的技巧。对于大多数应用中的组件来说，这通常不是必需的。但其对某些组件，尤其是可重用的组件库是很有用的。

###### 转发 refs 到 DOM 组件

**Ref 转发是一个可选特性，其允许某些组件接收 `ref`，并将其向下传递（换句话说，“转发”它）给子组件。**

##### Fragments

React 中的一个常见模式是一个组件返回多个元素。Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```

###### 短语法

你可以使用一种新的，且更简短的语法来声明 Fragments。它看起来像空标签：

```
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

###### 带 key 的 Fragments

使用显式 `<React.Fragment>` 语法声明的片段可能具有 key。一个使用场景是将一个集合映射到一个 Fragments 数组。

```
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // 没有`key`，React 会发出一个关键警告
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

`key` 是唯一可以传递给 `Fragment` 的属性。未来我们可能会添加对其他属性的支持，例如事件。

##### 高阶组件

高阶组件（HOC）是React中用于复用组件逻辑的一种高级技巧。HOC自身不是React API的一部分，它是一种基于React的组合特性而形成的设计模式。

具体而言，高阶组件是参数为组件，返回值为新组件的函数。

```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

组件是将props转换为UI，而高阶组件是将组件转换为另一个组件。

HOC 在 React 的第三方库中很常见，例如 Redux 的 [`connect`](https://github.com/reduxjs/react-redux/blob/master/docs/api/connect.md#connect) 和 Relay 的 [`createFragmentContainer`](http://facebook.github.io/relay/docs/en/fragment-container.html)。

##### 与第三方库协同

React 可以被用于任何 web 应用中。它可以被嵌入到其他应用，且需要注意，其他的应用也可以被嵌入到 React。

##### 深入 JSX

实际上，JSX 仅仅只是 `React.createElement(component, props, ...children)` 函数的语法糖。

###### 指定 React 元素类型

JSX 标签的第一部分指定了 React 元素的类型。

大写字母开头的 JSX 标签意味着它们是 React 组件。这些标签会被编译为对命名变量的直接引用，所以，当你使用 JSX `<Foo />` 表达式时，`Foo` 必须包含在作用域内。

###### React 必须在作用域内

由于 JSX 会编译为 `React.createElement` 调用形式，所以 `React` 库也必须包含在 JSX 代码作用域内。

###### 在 JSX 类型中使用点语法

在 JSX 中，你也可以使用点语法来引用一个 React 组件。当你在一个模块中导出许多 React 组件时，这会非常方便。例如，如果 `MyComponents.DatePicker` 是一个组件，你可以在 JSX 中直接使用：

```
import React from react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}

```

###### 用户定义的组件必须以大写字母开头

以小写字母开头的元素代表一个 HTML 内置组件，比如 `<div>` 或者 `<span>` 会生成相应的字符串 `'div'` 或者 `'span'` 传递给 `React.createElement`（作为参数）。大写字母开头的元素则对应着在 JavaScript 引入或自定义的组件，如 `<Foo />` 会编译为 `React.createElement(Foo)`。

我们建议使用大写字母开头命名自定义组件。如果你确实需要一个以小写字母开头的组件，则在 JSX 中使用它之前，必须将它赋值给一个大写字母开头的变量。

###### JSX 中的 Props

有多种方式可以在 JSX 中指定 props。

###### JavaScript 表达式作为 Props

你可以把包裹在 `{}` 中的 JavaScript 表达式作为一个 prop 传递给 JSX 元素。

`if` 语句以及 `for` 循环不是 JavaScript 表达式，所以不能在 JSX 中直接使用。但是，你可以用在 JSX 以外的代码中。

###### 字符串字面量

你可以将字符串字面量赋值给 prop. 如下两个 JSX 表达式是等价的：

```
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />
```

###### Props 默认值为 “True”

如果你没给 prop 赋值，它的默认值是 `true`。以下两个 JSX 表达式是等价的：

```
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

通常，我们不建议不传递 value 给 prop，因为这可能与 [ES6 对象简写](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#New_notations_in_ECMAScript_2015)混淆，`{foo}` 是 `{foo: foo}` 的简写，而不是 `{foo: true}`。这样实现只是为了保持和 HTML 中标签属性的行为一致。

###### 属性展开

如果你已经有了一个 props 对象，你可以使用展开运算符 `...` 来在 JSX 中传递整个 props 对象。以下两个组件是等价的：

```
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

###### JSX 中的子元素

包含在开始和结束标签之间的 JSX 表达式内容将作为特定属性 `props.children` 传递给外层组件。有几种不同的方法来传递子元素：

###### 字符串字面量

你可以将字符串放在开始和结束标签之间，此时 `props.children` 就只是该字符串。这对于很多内置的 HTML 元素很有用。例如：

```
<MyComponent>Hello world!</MyComponent>
```

这是一个合法的 JSX，`MyComponent` 中的 `props.children` 是一个简单的未转义字符串 `"Hello world!"`。因此你可以采用编写 HTML 的方式来编写 JSX。如下所示：

```
<div>This is valid HTML &amp; JSX at the same time.</div>
```

JSX 会移除行首尾的空格以及空行。与标签相邻的空行均会被删除，文本字符串之间的新行会被压缩为一个空格。

###### JSX 子元素

子元素允许由多个 JSX 元素组成。

###### JavaScript 表达式作为子元素

JavaScript 表达式可以被包裹在 `{}` 中作为子元素。

###### 函数作为子元素

通常，JSX 中的 JavaScript 表达式将会被计算为字符串、React 元素或者是列表。不过，`props.children` 和其他 prop 一样，它可以传递任意类型的数据，而不仅仅是 React 已知的可渲染类型。

###### 布尔类型、Null 以及 Undefined 将会忽略

`false`, `null`, `undefined`, and `true` 是合法的子元素。但它们并不会被渲染。

##### 性能优化

UI 更新需要昂贵的 DOM 操作，而 React 内部使用几种巧妙的技术以便最小化 DOM 操作次数。对于大部分应用而言，使用 React 时无需专门优化就已拥有高性能的用户界面。尽管如此，你仍然有办法来加速你的 React 应用。

###### 使用生产版本

如果你不能确定你的编译过程是否设置正确，你可以通过安装 [Chrome 的 React 开发者工具](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) 来检查。如果你浏览一个基于 React 生产版本的网站，图标背景会变成深色。如果你浏览一个基于 React 开发模式的网站，图标背景会变成红色

###### 单文件构建

###### Brunch

###### Browserify

###### Rollup

###### webpack

###### 使用 Chrome Performance 标签分析组件

###### 使用开发者工具中的分析器对组件进行分析

###### 虚拟化长列表

###### 避免调停

###### shouldComponentUpdate 的作用

###### 不可变数据的力量

##### Portals

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

```
ReactDOM.createPortal(child, container)
```

第一个参数（`child`）是任何[可渲染的 React 子元素](https://reactjs.bootcss.com/docs/react-component.html#render)，例如一个元素，字符串或 fragment。第二个参数（`container`）是一个 DOM 元素。

##### Profiler API

`Profiler` 测量渲染一个 React 应用多久渲染一次以及渲染一次的“代价”。 它的目的是识别出应用中渲染较慢的部分，或是可以使用[类似 memoization 优化](https://reactjs.bootcss.com/docs/hooks-faq.html#how-to-memoize-calculations)的部分，并从相关优化中获益。

###### 用法

`Profiler` 能添加在 React 树中的任何地方来测量树中这部分渲染所带来的开销。 它需要两个 prop ：一个是 `id`(string)，一个是当组件树中的组件“提交”更新的时候被React调用的回调函数 `onRender`(function)。

例如，为了分析 `Navigation` 组件和它的子代：

```
render(
  <App>
    <Profiler id="Navigation" onRender={callback}>      <Navigation {...props} />
    </Profiler>
    <Main {...props} />
  </App>
);
```

##### 不使用ES6

通常我们会用 JavaScript 的 `class` 关键字来定义 React 组件。如果你还未使用过 ES6，你可以使用 `create-react-class` 模块。

##### 不使用JSX的React

React 并不强制要求使用 JSX。当你不想在构建环境中配置有关 JSX 编译时，不在 React 中使用 JSX 会更加方便。

每个 JSX 元素只是调用 `React.createElement(component, props, ...children)` 的语法糖。因此，使用 JSX 可以完成的任何事情都可以通过纯 JavaScript 完成。

#####  协调

##### Diffing 算法

当对比两颗树时，React 首先比较两棵树的根节点。不同类型的根节点元素会有不同的形态。

###### 对比不同类型的元素

当根节点为不同类型的元素时，React 会拆卸原有的树并且建立起新的树。

###### 对比同一类型的元素

当对比两个相同类型的 React 元素时，React 会保留 DOM 节点，仅比对及更新有改变的属性。

###### 对比同类型的组件元素

当一个组件更新时，组件实例保持不变，这样 state 在跨越不同的渲染时保持一致。React 将更新该组件实例的 props 以跟最新的元素保持一致，并且调用该实例的 `componentWillReceiveProps()` 和 `componentWillUpdate()` 方法。

下一步，调用 `render()` 方法，diff 算法将在之前的结果以及新的结果中进行递归。

###### 对子节点进行递归

在默认条件下，当递归 DOM 节点的子元素时，React 会同时遍历两个子元素的列表；当产生差异时，生成一个 mutation。

在子元素列表末尾新增元素时，更新开销比较小。

##### Keys

为了解决以上问题，React 支持 `key` 属性。当子元素拥有 key 时，React 使用 key 来匹配原有树上的子元素以及最新树上的子元素。

###### 权衡

请谨记协调算法是一个实现细节。React 可以在每个 action 之后对整个应用进行重新渲染，得到的最终结果也会是一样的。在此情境下，重新渲染表示在所有组件内调用 `render` 方法，这不代表 React 会卸载或装载它们。React 只会基于以上提到的规则来决定如何进行差异的合并。

我们定期探索优化算法，让常见用例更高效地执行。在当前的实现中，可以理解为一棵子树能在其兄弟之间移动，但不能移动到其他位置。在这种情况下，算法会重新渲染整棵子树。

由于 React 依赖探索的算法，因此当以下假设没有得到满足，性能会有所损耗。

1. 该算法不会尝试匹配不同组件类型的子树。如果你发现你在两种不同类型的组件中切换，但输出非常相似的内容，建议把它们改成同一类型。在实践中，我们没有遇到这类问题。
2. Key 应该具有稳定，可预测，以及列表内唯一的特质。不稳定的 key（比如通过 `Math.random()` 生成的）会导致许多组件实例和 DOM 节点被不必要地重新创建，这可能导致性能下降和子组件中的状态丢失。

##### Refs and the DOM

Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。

在典型的 React 数据流中，[props](https://reactjs.bootcss.com/docs/components-and-props.html) 是父组件与子组件交互的唯一方式。要修改一个子组件，你需要使用新的 props 来重新渲染它。但是，在某些情况下，你需要在典型数据流之外强制修改子组件。被修改的子组件可能是一个 React 组件的实例，也可能是一个 DOM 元素。对于这两种情况，React 都提供了解决办法。

###### 何时使用 Refs

下面是几个适合使用 refs 的情况：

- 管理焦点，文本选择或媒体播放。
- 触发强制动画。
- 集成第三方 DOM 库。

##### Render Props

术语 [“render prop”](https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce) 是指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术

具有 render prop 的组件接受一个函数，该函数返回一个 React 元素并调用它而不是实现自己的渲染逻辑。

```
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```

使用 render prop 的库有 [React Router](https://reacttraining.com/react-router/web/api/Route/render-func)、[Downshift](https://github.com/paypal/downshift) 以及 [Formik](https://github.com/jaredpalmer/formik)。

##### 静态类型检查

像 [Flow](https://flow.org/) 和 [TypeScript](https://www.typescriptlang.org/) 等这些静态类型检查器，可以在运行前识别某些类型的问题。他们还可以通过增加自动补全等功能来改善开发者的工作流程。出于这个原因，我们建议在大型代码库中使用 Flow 或 TypeScript 来代替 `PropTypes`。

###### TypeScript

[TypeScript](https://www.typescriptlang.org/) 是一种由微软开发的编程语言。它是 JavaScript 的一个类型超集，包含独立的编译器。作为一种类型语言，TypeScript 可以在构建时发现 bug 和错误，这样程序运行时就可以避免此类错误。

###### 文件扩展名

在 React 中，你的组件文件大多数使用 `.js` 作为扩展名。在 TypeScript 中，提供两种文件扩展名：

`.ts` 是默认的文件扩展名，而 `.tsx` 是一个用于包含 `JSX` 代码的特殊扩展名。

##### 严格模式

`StrictMode` 是一个用来突出显示应用程序中潜在问题的工具。与 `Fragment` 一样，`StrictMode` 不会渲染任何可见的 UI。它为其后代元素触发额外的检查和警告。

注意：

严格模式检查仅在开发模式下运行；*它们不会影响生产构建*。

```
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```

`StrictMode` 目前有助于：

- [识别不安全的生命周期](https://reactjs.bootcss.com/docs/strict-mode.html#identifying-unsafe-lifecycles)
- [关于使用过时字符串 ref API 的警告](https://reactjs.bootcss.com/docs/strict-mode.html#warning-about-legacy-string-ref-api-usage)
- [关于使用废弃的 findDOMNode 方法的警告](https://reactjs.bootcss.com/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)
- [检测意外的副作用](https://reactjs.bootcss.com/docs/strict-mode.html#detecting-unexpected-side-effects)
- [检测过时的 context API](https://reactjs.bootcss.com/docs/strict-mode.html#detecting-legacy-context-api)

##### 使用 PropTypes 进行类型检查

注意：

自 React v15.5 起，`React.PropTypes` 已移入另一个包中。请使用 [`prop-types` 库](https://www.npmjs.com/package/prop-types) 代替。

随着你的应用程序不断增长，你可以通过类型检查捕获大量错误。对于某些应用程序来说，你可以使用 [Flow](https://flow.org/) 或 [TypeScript](https://www.typescriptlang.org/) 等 JavaScript 扩展来对整个应用程序做类型检查。但即使你不使用这些扩展，React 也内置了一些类型检查的功能。要在组件的 props 上进行类型检查，你只需配置特定的 `propTypes` 属性。

##### 非受控组件

在大多数情况下，我们推荐使用 [受控组件](https://reactjs.bootcss.com/docs/forms.html#controlled-components) 来处理表单数据。在一个受控组件中，表单数据是由 React 组件来管理的。另一种替代方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理。

要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以 [使用 ref](https://reactjs.bootcss.com/docs/refs-and-the-dom.html) 来从 DOM 节点中获取表单数据。

##### Web Components

React 和 [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components) 为了解决不同的问题而生。Web Components 为可复用组件提供了强大的封装，而 React 则提供了声明式的解决方案，使 DOM 与数据保持同步。两者旨在互补。作为开发人员，可以自由选择在 Web Components 中使用 React，或者在 React 中使用 Web Components，或者两者共存。

##### React顶层API

`React` 是 React 库的入口。如果你通过使用 `<script>` 标签的方式来加载 React，则可以通过 `React` 全局变量对象来获得 React 的顶层 API。当你使用 ES6 与 npm 时，可以通过编写 `import React from 'react'` 来引入它们。当你使用 ES5 与 npm 时，则可以通过编写 `var React = require('react')` 来引入它们。

###### 组件

使用 React 组件可以将 UI 拆分为独立且复用的代码片段，每部分都可独立维护。你可以通过子类 `React.Component` 或 `React.PureComponent` 来定义 React 组件。

`React.PureComponent` 与 [`React.Component`](https://reactjs.bootcss.com/docs/react-api.html#reactcomponent) 很相似。两者的区别在于 [`React.Component`](https://reactjs.bootcss.com/docs/react-api.html#reactcomponent) 并未实现 [`shouldComponentUpdate()`](https://reactjs.bootcss.com/docs/react-component.html#shouldcomponentupdate)，而 `React.PureComponent` 中以浅层对比 prop 和 state 的方式来实现了该函数。

如果赋予 React 组件相同的 props 和 state，`render()` 函数会渲染相同的内容，那么在某些情况下使用 `React.PureComponent` 可提高性能。

注意

`React.PureComponent` 中的 `shouldComponentUpdate()` 仅作对象的浅层比较。如果对象中包含复杂的数据结构，则有可能因为无法检查深层的差别，产生错误的比对结果。仅在你的 props 和 state 较为简单时，才使用 `React.PureComponent`，或者在深层数据结构发生变化时调用 [`forceUpdate()`](https://reactjs.bootcss.com/docs/react-component.html#forceupdate) 来确保组件被正确地更新。你也可以考虑使用 [immutable 对象](https://facebook.github.io/immutable-js/)加速嵌套数据的比较。

此外，`React.PureComponent` 中的 `shouldComponentUpdate()` 将跳过所有子组件树的 prop 更新。因此，请确保所有子组件也都是“纯”的组件。

```
const MyComponent = React.memo(function MyComponent(props) {
  /* 使用 props 渲染 */
});
```

`React.memo` 为[高阶组件](https://reactjs.bootcss.com/docs/higher-order-components.html)。它与 [`React.PureComponent`](https://reactjs.bootcss.com/docs/react-api.html#reactpurecomponent) 非常相似，但只适用于函数组件，而不适用 class 组件。

如果你的函数组件在给定相同 props 的情况下渲染相同的结果，那么你可以通过将其包装在 `React.memo` 中调用，以此通过记忆组件渲染结果的方式来提高组件的性能表现。这意味着在这种情况下，React 将跳过渲染组件的操作并直接复用最近一次渲染的结果。

`React.memo` 仅检查 props 变更。如果函数组件被 `React.memo` 包裹，且其实现中拥有 [`useState`](https://reactjs.bootcss.com/docs/hooks-state.html) 或 [`useContext`](https://reactjs.bootcss.com/docs/hooks-reference.html#usecontext) 的 Hook，当 context 发生变化时，它仍会重新渲染。

###### 创建React元素

我们建议[使用 JSX](https://reactjs.bootcss.com/docs/introducing-jsx.html) 来编写你的 UI 组件。每个 JSX 元素都是调用 [`React.createElement()`](https://reactjs.bootcss.com/docs/react-api.html#createelement) 的语法糖。一般来说，如果你使用了 JSX，就不再需要调用以下方法。

- [`createElement()`](https://reactjs.bootcss.com/docs/react-api.html#createelement)  创建并返回指定类型的新 [React 元素](https://reactjs.bootcss.com/docs/rendering-elements.html)。
- [`createFactory()`](https://reactjs.bootcss.com/docs/react-api.html#createfactory) 返回用于生成指定类型 React 元素的函数。

###### 转换元素

`React` 提供了几个用于操作元素的 API：

- [`cloneElement()`](https://reactjs.bootcss.com/docs/react-api.html#cloneelement) 以 `element` 元素为样板克隆并返回新的 React 元素。返回元素的 props 是将新的 props 与原始元素的 props 浅层合并后的结果。新的子元素将取代现有的子元素，而来自原始元素的 `key` 和 `ref` 将被保留

```
<element.type {...element.props} {...props}>{children}</element.type>
```

- [`isValidElement()`](https://reactjs.bootcss.com/docs/react-api.html#isvalidelement) 验证对象是否为 React 元素，返回值为 `true` 或 `false`。
- [`React.Children`](https://reactjs.bootcss.com/docs/react-api.html#reactchildren) `React.Children` 提供了用于处理 `this.props.children` 不透明数据结构的实用方法。

###### Fragments

React还提供了用于减少不必要嵌套的组件。

React.Fragment 组件能够在不额外创建 DOM 元素的情况下，让 `render()` 方法中返回多个元素。

###### Refs

- [`React.createRef`](https://reactjs.bootcss.com/docs/react-api.html#reactcreateref)  创建一个能够通过 ref 属性附加到 React 元素的 [ref](https://reactjs.bootcss.com/docs/refs-and-the-dom.html)。
- [`React.forwardRef`](https://reactjs.bootcss.com/docs/react-api.html#reactforwardref) `React.forwardRef` 会创建一个React组件，这个组件能够将其接受的 [ref](https://reactjs.bootcss.com/docs/refs-and-the-dom.html) 属性转发到其组件树下的另一个组件中。

###### Suspense

Suspense 使得组件可以“等待”某些操作结束后，再进行渲染。目前，Suspense 仅支持的使用场景是：[通过 `React.lazy` 动态加载组件](https://reactjs.bootcss.com/docs/code-splitting.html#reactlazy)。它将在未来支持其它使用场景，如数据获取等。

- [`React.lazy`](https://reactjs.bootcss.com/docs/react-api.html#reactlazy) `React.lazy()` 允许你定义一个动态加载的组件。这有助于缩减 bundle 的体积，并延迟加载在初次渲染时未用到的组件。
- [`React.Suspense`](https://reactjs.bootcss.com/docs/react-api.html#reactsuspense)  可以指定加载指示器（loading indicator），以防其组件树中的某些子组件尚未具备渲染条件。目前，懒加载组件是 `<React.Suspense>` 支持的**唯一**用例

###### Hook

*Hook* 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。Hook 拥有[专属文档章节](https://reactjs.bootcss.com/docs/hooks-intro.html)和单独的 API 参考文档：

- [基础 Hook](https://reactjs.bootcss.com/docs/hooks-reference.html#basic-hooks)

  - [`useState`](https://reactjs.bootcss.com/docs/hooks-reference.html#usestate)
  - [`useEffect`](https://reactjs.bootcss.com/docs/hooks-reference.html#useeffect)
  - [`useContext`](https://reactjs.bootcss.com/docs/hooks-reference.html#usecontext)

- [额外的 Hook](https://reactjs.bootcss.com/docs/hooks-reference.html#additional-hooks)

  - [`useReducer`](https://reactjs.bootcss.com/docs/hooks-reference.html#usereducer)
  - [`useCallback`](https://reactjs.bootcss.com/docs/hooks-reference.html#usecallback)
  - [`useMemo`](https://reactjs.bootcss.com/docs/hooks-reference.html#usememo)
  - [`useRef`](https://reactjs.bootcss.com/docs/hooks-reference.html#useref)
  - [`useImperativeHandle`](https://reactjs.bootcss.com/docs/hooks-reference.html#useimperativehandle)
  - [`useLayoutEffect`](https://reactjs.bootcss.com/docs/hooks-reference.html#uselayouteffect)
  - [`useDebugValue`](https://reactjs.bootcss.com/docs/hooks-reference.html#usedebugvalue)

  ##### React.Component

  ###### 组件的生命周期

  ###### 挂载  当组件实例被创建并插入DOM中时，其生命周期调用顺序如下：

  1. constructor()
  2. static getDerivedStateFromProps()
  3. render()
  4. componentDidMount()

  注意：下述生命周期方法即将过时，在新代码中应该避免使用它们：

  UNSAFE_componentWillMount()

  ###### 更新 当组件的props或state发生变化时会触发更新。组件更新的生命周期调用顺序如下：

  1. static getDerivedStateFromProps()
  2. shouldComponentUpdate()
  3. render()
  4. getSnapshotBeforeUpdate()
  5. componentDidUpdate()

  注意 下述方法即将过时，在新代码中应该避免使用它们：

  UNSAFE_componentWillUpdate()

  UNSAFE_componentWillReceiveProps()

###### 卸载 当组件从DOM中移除时会调用如下方法：

componentWillUnmount()

###### 错误处理 当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：

static getDerivedStateFromError()

componentDidCatch()

###### 其他APIs

组件还提供了一些额外的API:

setState()

forceUpdate()

###### class属性

defaultProps

displayName

###### 实例属性

props

state

##### React 术语词汇表

###### 单页面应用

单页面应用(single-page application)，是一个应用程序，它可以加载单个 HTML 页面，以及运行应用程序所需的所有必要资源（例如 JavaScript 和 CSS）。与页面或后续页面的任何交互，都不再需要往返 server 加载资源，即页面不会重新加载。

###### ES6, ES2015, ES2016 等

这些首字母缩写都是指 ECMAScript 语言规范标准的最新版本，JavaScript 语言是此标准的一个实现。其中 ES6 版本（也称为 ES2015）包括对前面版本的许多补充，例如：箭头函数、class、模板字面量、`let` 和 `const` 语句。

###### Compiler（编译器）

JavaScript compiler 接收 JavaScript 代码，然后对其进行转换，最终返回不同格式的 JavaScript 代码。最为常见的使用示例是，接收 ES6 语法，然后将其转换为旧版本浏览器能够解释执行的语法。[Babel](https://babeljs.io/) 是 React 最常用的 compiler。

###### Bundler（打包工具）

bundler 会接收写成单独模块（通常有数百个）的 JavaScript 和 CSS 代码，然后将它们组合在一起，最终生成出一些为浏览器优化的文件。常用的打包 React 应用的工具有 [webpack](https://webpack.js.org/) 和 [Browserify](http://browserify.org/)。

###### Package 管理工具

package 管理工具，是帮助你管理项目依赖的工具。[npm](https://www.npmjs.com/) 和 [Yarn](https://yarnpkg.com/) 是两个常用的管理 React 应用依赖的 package 管理工具。它们都是使用了相同 npm package registry 的客户端。

###### CDN

CDN 代表内容分发网络（Content Delivery Network）。CDN 会通过一个遍布全球的服务器网络来分发缓存的静态内容。

###### JSX

JSX 是一个 JavaScript 语法扩展。它类似于模板语言，但它具有 JavaScript 的全部能力。JSX 最终会被编译为 `React.createElement()` 函数调用，返回称为 “React 元素” 的普通 JavaScript 对象。

React DOM 使用 camelCase（驼峰式命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。例如，HTML 的 `tabindex` 属性变成了 JSX 的 `tabIndex`。而 `class` 属性则变为 `className`，这是因为 `class` 是 JavaScript 中的保留字

###### 元素

React 元素是构成 React 应用的基础砖块。人们可能会把元素与广为人知的“组件”概念相互混淆。元素描述了你在屏幕上想看到的内容。React 元素是不可变对象。

```
const element = <h1>Hello, world</h1>;
```

通常我们不会直接使用元素，而是从组件中返回元素。

###### 组件

React 组件是可复用的小的代码片段，它们返回要在页面中渲染的 React 元素。React 组件的最简版本是，一个返回 React 元素的普通 JavaScript 函数

###### props

`props` 是 React 组件的输入。它们是从父组件向下传递给子组件的数据。

记住，`props` 是只读的。不应以任何方式修改它们

###### `props.children`

每个组件都可以获取到 `props.children`。它包含组件的开始标签和结束标签之间的内容。

###### state

当组件中的一些数据在某些时刻发生变化时，这时就需要使用 `state` 来跟踪状态。例如，`Checkbox` 组件可能需要 `isChecked` 状态，而 `NewsFeed` 组件可能需要跟踪 `fetchedPosts` 状态。

`state` 和 `props` 之间最重要的区别是：`props` 由父组件传入，而 `state` 由组件本身管理。组件不能修改 `props`，但它可以修改 `state`。

对于所有变化数据中的每个特定部分，只应该由一个组件在其 state 中“持有”它。不要试图同步来自于两个不同组件的 state。相反，应当将其[提升](https://reactjs.bootcss.com/docs/lifting-state-up.html)到最近的共同祖先组件中，并将这个 state 作为 props 传递到两个子组件。

###### 周期方法

生命周期方法，用于在组件不同阶段执行自定义功能。在组件被创建并插入到 DOM 时（即[挂载中阶段（mounting）](https://reactjs.bootcss.com/docs/react-component.html#mounting)），组件更新时，组件取消挂载或从 DOM 中删除时，都有可以使用的生命周期方法。

###### 受控组件vs 非受控组件

React 有两种不同的方式来处理表单输入。

如果一个 input 表单元素的值是由 React 控制，就其称为*受控组件*。当用户将数据输入到受控组件时，会触发修改状态的事件处理器，这时由你的代码来决定此输入是否有效（如果有效就使用更新后的值重新渲染）。如果不重新渲染，则表单元素将保持不变。

一个*非受控组件*，就像是运行在 React 体系之外的表单元素。当用户将数据输入到表单字段（例如 input，dropdown 等）时，React 不需要做任何事情就可以映射更新后的信息。然而，这也意味着，你无法强制给这个表单字段设置一个特定值。

在大多数情况下，你应该使用受控组件。

###### key

“key” 是在创建元素数组时，需要用到的一个特殊字符串属性。key 帮助 React 识别出被修改、添加或删除的 item。应当给数组内的每个元素都设定 key，以使元素具有固定身份标识。

只需要保证，在同一个数组中的兄弟元素之间的 key 是唯一的。而不需要在整个应用程序甚至单个组件中保持唯一。

不要将 `Math.random()` 之类的值传递给 key。重要的是，在前后两次渲染之间的 key 要具有“固定身份标识”的特点，以便 React 可以在添加、删除或重新排序 item 时，前后对应起来。理想情况下，key 应该从数据中获取，对应着唯一且固定的标识符，例如 `post.id`。

###### Ref

React 支持一个特殊的、可以附加到任何组件上的 `ref` 属性。此属性可以是一个由 [`React.createRef()` 函数](https://reactjs.bootcss.com/docs/react-api.html#reactcreateref)创建的对象、或者一个回调函数、或者一个字符串（遗留 API）。当 `ref` 属性是一个回调函数时，此函数会（根据元素的类型）接收底层 DOM 元素或 class 实例作为其参数。这能够让你直接访问 DOM 元素或组件实例。

谨慎使用 ref。如果你发现自己经常使用 ref 来在应用中“实现想要的功能”，你可以考虑去了解一下[自上而下的数据流](https://reactjs.bootcss.com/docs/lifting-state-up.html)。

###### 事件

使用 React 元素处理事件时，有一些语法上差异：

- React 事件处理器使用 camelCase（驼峰式命名）而不使用小写命名。
- 通过 JSX，你可以直接传入一个函数，而不是传入一个字符串，来作为事件处理器。

###### 协调

当组件的 props 或 state 发生变化时，React 通过将最新返回的元素与原先渲染的元素进行比较，来决定是否有必要进行一次实际的 DOM 更新。当它们不相等时，React 才会更新 DOM。这个过程被称为“协调”。

###### 什么是 Virtual DOM？

Virtual DOM 是一种编程概念。在这个概念里， UI 以一种理想化的，或者说“虚拟的”表现形式被保存于内存中，并通过如 ReactDOM 等类库使之与“真实的” DOM 同步。这一过程叫做[协调](https://reactjs.bootcss.com/docs/reconciliation.html)。

这种方式赋予了 React 声明式的 API：您告诉 React 希望让 UI 是什么状态，React 就确保 DOM 匹配该状态。这使您可以从属性操作、事件处理和手动 DOM 更新这些在构建应用程序时必要的操作中解放出来。

与其将 “Virtual DOM” 视为一种技术，不如说它是一种模式，人们提到它时经常是要表达不同的东西。在 React 的世界里，术语 “Virtual DOM” 通常与 [React 元素](https://reactjs.bootcss.com/docs/rendering-elements.html)关联在一起，因为它们都是代表了用户界面的对象。而 React 也使用一个名为 “fibers” 的内部对象来存放组件树的附加信息。上述二者也被认为是 React 中 “Virtual DOM” 实现的一部分。

###### Shadow DOM 和 Virtual DOM 是一回事吗？

不，他们不一样。Shadow DOM 是一种浏览器技术，主要用于在 web 组件中封装变量和 CSS。Virtual DOM 则是一种由 Javascript 类库基于浏览器 API 实现的概念。

###### 什么是 “React Fiber”？

Fiber 是 React 16 中新的协调引擎。它的主要目的是使 Virtual DOM 可以进行增量式渲染。

###### `setState` 实际做了什么？

`setState()` 会对一个组件的 `state` 对象安排一次更新。当 state 改变了，该组件就会重新渲染。

###### `state` 和 `props` 之间的区别是什么？

[`props`](https://reactjs.bootcss.com/docs/components-and-props.html)（“properties” 的缩写）和 [`state`](https://reactjs.bootcss.com/docs/state-and-lifecycle.html) 都是普通的 JavaScript 对象。它们都是用来保存信息的，这些信息可以控制组件的渲染输出，而它们的一个重要的不同点就是：`props` 是传递*给*组件的（类似于函数的形参），而 `state` 是在组件*内*被组件自己管理的（类似于在一个函数内声明的变量）。

###### 为什么 `setState` 给了我一个错误的值？

在 React 中，`this.props` 和 `this.state` 都代表着*已经被渲染了的*值，即当前屏幕上显示的值。

调用 `setState` 其实是异步的 —— 不要指望在调用 `setState` 之后，`this.state` 会立即映射为新的值。如果你需要基于当前的 state 来计算出新的值，那你应该传递一个函数，而不是一个对象。

###### 我应该如何更新那些依赖于当前的 state 的 state 呢？

给 `setState` 传递一个函数，而不是一个对象，就可以确保每次的调用都是使用最新版的 state（见下面的说明）。

###### 给 `setState` 传递一个对象与传递一个函数的区别是什么？

传递一个函数可以让你在函数内访问到当前的 state 的值。因为 `setState` 的调用是分批的，所以你可以链式地进行更新，并确保它们是一个建立在另一个之上的，这样才不会发生冲突。

###### `setState` 什么时候是异步的？

目前，在事件处理函数内部的 `setState` 是异步的。

例如，如果 `Parent` 和 `Child` 在同一个 click 事件中都调用了 `setState` ，这样就可以确保 `Child` 不会被重新渲染两次。取而代之的是，React 会将该 state “冲洗” 到浏览器事件结束的时候，再统一地进行更新。这种机制可以在大型应用中得到很好的性能提升。

这只是一个实现的细节，所以请不要直接依赖于这种机制。在以后的版本当中，React 会在更多的情况下静默地使用 state 的批更新机制。

###### 为什么 React 不同步地更新 `this.state`？

如前面章节解释的那样，在开始重新渲染之前，React 会有意地进行“等待”，直到所有在组件的事件处理函数内调用的 `setState()` 完成之后。这样可以通过避免不必要的重新渲染来提升性能。

但是，你可能还是会想，为什么 React 不能立即更新 `this.state`，而不对组件进行重新渲染呢。

主要有两个原因：

- 这样会破坏掉 `props` 和 `state` 之间的一致性，造成一些难以 debug 的问题。
- 这样会让一些我们正在实现的新功能变得无法实现。

###### 怎样阻止函数被调用太快或者太多次？

如果你有一个 `onClick` 或者 `onScroll` 这样的事件处理器，想要阻止回调被触发的太快，那么可以限制执行回调的速度，可以通过以下几种方式做到这点：

- **节流**：基于时间的频率来进行抽样更改 (例如 [`_.throttle`](https://lodash.com/docs#throttle))
- **防抖**：一段时间的不活动之后发布更改 (例如 [`_.debounce`](https://lodash.com/docs#debounce))
- **`requestAnimationFrame` 节流**：基于 requestAnimationFrame 的抽样更改 (例如 [raf-schd](https://reactjs.bootcss.com/docs/[`raf-schd`](https://github.com/alexreardon/raf-schd)))

###### 节流

节流阻止函数在给定时间窗口内被调不能超过一次。

###### 防抖

防抖确保函数不会在上一次被调用之后一定量的时间内被执行。当必须进行一些费时的计算来响应快速派发的事件时（比如鼠标滚动或键盘事件时），防抖是非常有用的。

###### `requestAnimationFrame` 节流

[`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) 是在浏览器中排队等待执行的一种方法，它可以在呈现性能的最佳时间执行。

