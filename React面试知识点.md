##### React面试知识点

##### 1.什么是声明式编程

声明式编程是一种编程范式，它关注的是你要做什么，而不是如何做。它表达逻辑而不显式地定义步骤。这意味着我们需要根据逻辑的计算来声明要显示的组件。它没有描述控制流步骤。声明式编程的例子有HTML、SQL等。

##### 2.声明式编程VS命令式编程

声明式编程的编写方式描述了应该做什么，而命令式编程描述了如何做。在声明式编程中，让编译器决定如何做事情。声明性程序很容易推理，因为代码本身描述了它在做什么。

下面是一个例子，数组中的每个元素都乘以 `2`，我们使用声明式`map`函数，让编译器来完成其余的工作，而使用命令式，需要编写所有的流程步骤。

const numbers = [1,2,3,4,5];

// 声明式

const doubleWithDec = numbers.map(number => number * 2);

console.log(doubleWithDec)

// 命令式

const doubleWithImp = [];

for(let i=0; i<numbers.length; i++) {

​    const numberdouble = numbers[i] * 2;

​    doubleWithImp.push(numberdouble)

}

console.log(doubleWithImp)

##### 3.什么是函数式编程

函数式编程是声明式编程的一部分。javascript中的函数是第一类公民，这意味着函数是数据，你可以像保存变量一样在应用程序中保存、检索和传递这些函数。

函数式编程有些核心的概念，如下：

不可变性（Immutability）

纯函数（Pure Functions）

数据转换（Data Transformations）

高阶函数（Higher-Order Functions）

递归

组合

不可变性（Immutability）

不可变性意味着不可改变。在函数式编程中，你无法更改数据，也不能更改。如果要改变或更改数据，则必须复制数据副本来更改。

###### 纯函数

纯函数是始终接受一个或多个参数并计算参数并返回数据或函数的函数。它没有副作用，例如设置全局状态，更改应用程序状态，它总是将参数视为不可改变数据。

使用纯函数，它接受参数，基于参数计算，返回一个新对象而不修改参数。

###### 数据转换

讲了很多关于不可变性的内容，如果数据是不可变的，我们如何改变数据。如上所述，我们总是生成原始数据的转换副本，而不是直接更改原始数据。再介绍一些Javascript内置函数，当然还有很多其他的函数。所有这些函数都不改变现有的数据，而是返回新的数组或对象。

###### 高阶函数

高阶函数是将函数作为参数或返回函数的函数，或者有时它们都有。这些高阶函数可以操纵其他函数。

Array.map , Array.filter 和 Array.reduce 是高阶函数，因为它们将函数作为参数。

###### 递归

递归是一种函数在满足一定条件之前调用自身的技术。只要可能，最好使用递归而不是循环。你必须注意这一点，浏览器不能处理太多递归和抛出错误。

###### 组合

在React中，我们将功能划分为小型可重用的纯函数，我们必须将所有这些可重用的函数放在一起，最终使其成为产品。将所有较小的函数组合成更大的函数，最终，得到一个应用程序，这称为组合。

实现组合有许多不同方法。从JavaScript中了解到的一种常见方法是链接。链接是一种使用点表示法调用前一个函数的返回值的函数的方法。

##### 4.什么是React

React是一个简单的JavaScript UI库，用于构建高效、快速的用户界面。它是一个轻量级库，因此很受欢迎。它遵循组件设计模式、声明式编程范式和函数式编程概念，以使前端应用程序更高效。它使用虚拟DOM来有效地操作DOM。它遵循从高阶组件到低阶组件的单向数据流。

##### 5.React 与 Angular 有何不同？

Angular是一个成熟的MVC框架，带有很多特定的特性，比如服务、指令、模板、模块、解析器等等。React是一个非常轻量级的库，它只关注MVC的视图部分。

Angular遵循两个方向的数据流，而React遵循从上到下的单向数据流。React在开发特性时给了开发人员很大的自由，例如，调用API的方式、路由等等。我们不需要包括路由器库，除非我们需要它在我们的项目。

##### 6.什么是Virtual DOM及其工作原理

React使用Virtual DOM来更新真正的DOM，从而提高效率和速度。

###### 什么是Virtual DOM

浏览器遵循HTML指令来构造文档对象模型（DOM）。当浏览器加载HTML并呈现用户界面时，HTML文档中的所有元素都变成DOM元素。

DOM是从根元素开始的元素层次结构。当在浏览器加载这个HTML时，所有这些HTML元素都被转换成DOM元素。当涉及到SPA应用程序时，首次加载index.html，并在index.html本身中加载更新后的数据或另一个html。当用户浏览站点时，我们使用新内容更新相同的index.html。每当DOM发生更改时，浏览器都需要重新计算CSS、进行布局并重新绘制web页面。

React使用Virtual DOM有效地重建DOM。对于我们来说，这使得DOM操作的一项非常复杂和耗时的任务变得更加容易。React从开发人员那里抽象出所有这些，以便在Virtual DOM的帮助下构建高效的UI。

###### 虚拟DOM是如何工作的

虚拟DOM只不过是真实DOM的javascript对象表示。与更新真实DOM相比，更新javascript对象更容易，更快捷。

React将整个DOM副本保存为虚拟DOM。每当有更新时，它都会维护两个虚拟DOM，以比较之前的状态和当前状态，并确定哪些对象已被更改。现在，它通过比较两个虚拟DOM差异，并将这些变化更新到实际DOM。一旦真正的DOM更新，它也会更新UI。

##### 7.什么是JSX

JSX是javascript的语法扩展。它就像一个拥有javascript全部 功能的模板语言。它生成React元素，这些元素将在DOM中呈现。React建议在组件使用JSX。在JSX中，我们结合了javascript和HTML,并生成了可以在DOM中呈现的react元素。

##### 8.组件和不同类型

React中一切都是组件。我们通常将应用程序的整个逻辑分解为小的单个部分。我们将每个单独的部分称为组件。通常，组件是一个javascript函数，它接受输入，处理它并返回在UI中呈现的React元素。

在React中有不同类型的组件。

###### 函数/无状态/展示组件

函数或无状态组件是一个纯函数，它可接受参数，并返回react元素。这些都是没有任何副作用的纯函数。这些组件没有状态或生命周期方法。

###### 类/有状态组件

类或有状态组件具有状态和生命周期方可能通过setState()方法更改组件的状态。类组件是通过扩展React创建的。它在构造函数中初始化，也可能有子组件。

###### 受控组件

受控组件是在React中处理输入表单的一种技术。表单元素通常维护它们自己的状态，而React则在组件的状态属性中维护状态。我们可以将两者结合起来控制输入表单。这称为受控组件。因此，在受控组件表单中，数据由React组件处理。

###### 非受控组件

大多数情况下，建议使用受控组件。有一种称为非受控组件的方法可以通过使用Ref来处理表单数据。在非受控组件中，Ref用于直接从DOM访问表单值，而不是事件处理程序。

###### 容器组件

容器组件是处理获取数据、订阅redux存储等的组件。它们包含展示组件和其他容器组件，但是里面从来没有html。

###### 高阶组件

高阶组件是将组件作为参数并生成另一个组件的组件。Redux connect是高阶组件的示例。这是一种用于生成可重用组件的强大技术。

##### 9.Props和State

Props是只读属性，传递给组件以呈现UI和状态。

##### 10.什么是PropTypes

PropTypes为组件提供类型检查，并为其他开发人员提供很好的文档。

##### 11.如何更新状态以及如何不更新

不应该直接修改状态。可以在构造函数中定义状态值。直接使用状态不会触发重新渲染。React使用this.setState()时合并状态。

使用this.setState()接受函数更安全，因为更新的props和状态是异步的。

##### 12.组件生命周期方法

###### **componentWillMount()**

在渲染前调用,在客户端也在服务端，它只发生一次。

###### **componentDidMount()**

在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过`this.getDOMNode()`来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用`setTimeout`, `setInterval`或者发送AJAX请求等操作(防止异部操作阻塞UI)。

###### componentWillReceiveProps()

在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。

###### **shouldComponentUpdate()**

返回一个布尔值。在组件接收到新的`props`或者`state`时被调用。在初始化时或者使用`forceUpdate`时不被调用。 可以在你确认不需要更新组件时使用。

###### **componentWillUpdate()**

在组件接收到新的`props`或者`state`但还没有`render`时被调用。在初始化时不会被调用。

###### **componentDidUpdate()**

在组件完成更新后立即调用。在初始化时不会被调用。

###### **componentWillUnMount()**

件从 DOM 中移除的时候立刻被调用。

###### **getDerivedStateFromError()**

这个生命周期方法在**ErrorBoundary**类中使用。实际上，如果使用这个生命周期方法，任何类都会变成`ErrorBoundary`。这用于在组件树中出现错误时呈现回退UI，而不是在屏幕上显示一些奇怪的错误。

###### **componentDidCatch()**

这个生命周期方法在ErrorBoundary类中使用。实际上，如果使用这个生命周期方法，任何类都会变成ErrorBoundary。这用于在组件树中出现错误时记录错误。

##### 13.超越继承的组合

在React中，我们总是使用组合而不是继承。我们已经在函数式编程部分讨论了什么是组合。这是一种结合简单的可重用函数来生成高阶组件的技术。

##### 14.如何在React中应用样式

将样式应用于React组件有三种方法。

###### 外部样式表

在此方法中，可以将外部样式表导入组件使用类中。但是你应该使用className而不是class来为React元素应用样式。

###### 内联样式

style={{backgroundColor:'orange'}}

###### 定义样式对象并使用它

因为我们将javascript对象传递给`style`属性，所以我们可以在组件中定义一个`style`对象并使用它。

##### 15.什么是Redux及其工作原理

Redux是React的一个状态管理库，它基于flux。Redux简化了React中的单向数据流。Redux将状态管理完全从React中抽象出来。

###### 它是如何工作的

在React中，组件连接到redux,如果要访问redux,需要派出一个包含id和负载（payload）的action。action中的payload是可选的，action将其转发给reducer。

当reducer收到action时，通过switch...case语法比较action中type。匹配时，更新对应的内容返回新的state。

当Redux状态更改时，连接到Redux的组件将接收新的状态作为props。当组件接收到这些props时，它将进入更新阶段并重新渲染UI。

###### Redux循环细节

![img](https://user-gold-cdn.xitu.io/2019/5/30/16b0804b978a975e?imageslim)

###### 组件如何与redux进行连接

mapStateToProps:此函数将state映射到props上，因此只要state发生变化，新state会重新映射到props。这是订阅store的方式。

mapDispatchToProps:此函数用于将action creators 绑定到你的props。

connect和bindActionCreators来自redux。前者用于连接store,后者用于将action creators 绑定到你的props。

##### 16.什么是React Router Dom 及其工作原理

react-router-dom 是应用程序中路由的库。React库中没有路由功能，需要单独安装react-router-dom。

react-router-dom 提供两个路由器BrowserRouter和HashRouter。前者基于rul的pathname段，后者基于hash段。

###### react-router-dom组件

BrowserRouter和HashRouter 是路由器。

Route用于路由匹配。

Link组件用于在应用程序中创建链接。它将在HTML中渲染为锚标记。

NavLink是突出显示当前活动链接的特殊链接。

Switch不是必需的，但在组合路由时很有用。

Redirect用于强制路由重定向。

##### 17.什么是错误边界（ErrorBoundary）

在React中，我们通常有一个组件树。如果任何一个组件发生错误，它将破坏整个组件树。没有办法捕捉这些错误，我们可以用错误边界优雅地处理这些错误。

错误边界有两个作用：

  1)如果发生错误，显示回退UI

  2)记录错误

##### 18.什么是Fragments

在React中，我们需要有一个父元素，同时从组件返回React元素。有时在DOM中添加额外的节点会很烦人。使用Fragments，我们不需要在DOM中添加额外的节点。我们只需要用React.Fragment或简写<>来包裹内容就行了。

##### 19.什么是传送门（Portals）

默认情况下，所有子组件都在UI上呈现，具体取决于组件层次结构。Portal提供了一种将子节点渲染到存在于父组件以外的DOM节点的优秀的方案。

##### 20.什么是上下文

有时我们必须将props传递给组件树，即使所有中间组件都不需要这些props。上下文是一种传递props的方法，而不用再每一层传递组件树。

##### 21.什么是Hooks

Hooks是React版本16.8中的新功能。请记住，我们不能在函数组件中使用state,因为它们不是类组件。Hooks让我们在函数组件中可以使用state和其他功能。

下面是Hooks的基本规则：

  1）Hooks应该在外层使用，不应该在循环，条件或嵌套函数中使用

  2）Hooks应该只在函数组件中使用。

##### 22.如何提高性能

  1）适当地使用shouldComponentUpdate生命周期方法。它避免了子组件的不必要的渲染。如果树中有100个组件，则不重新渲染整个组件树来提高应用程序性能。

 2）使用create-react-app 来构建项目，这会创建整个项目结构，并进行大量优化。

  3）不可变性是提高性能的关键。不要对数据进行修改，而是始终在现有集合的基础上创建新的集合，以保持尽可能少的复制，从而提高性能。

  4）在显示列表或表格时始终使用keys,这会让React的更新速度更快。

  5）代码分离是将代码插入到单独的文件中，只加载模块或部分所需的文件的 技术。

##### 23.如何在重新加载页面时保留数据

单页应用程序首先在DOM中加载index.html，然后在用户浏览页面时加载内容，或者从同一index.html中的后端API获取任何数据。

如果通过点击浏览器中的重新加载按钮重新加载页面index.html，整个React应用程序将重新加载，我们将丢失应用程序的状态。如何保留应用状态？

每当重新加载应用程序时，我们使用浏览器locaStorage来保存应用程序的状态。我们将整个存储数据保存在localStorage中，每当有页面刷新或重新加载时，我们从localStorage加载状态。

##### 24.如何在React进行API调用

我们使用redux-thunk在React中调用API。因为reduce是纯函数，所以没有副作用。

因此，我们必须使用redux-thunk从action creators 那里进行API调用。Action creator派发一个action,将来自API的数据放入action的payload中。

redux-thunk 是一个中间件。一旦它被引入到项目中，每次派发一个action时，都会通过thunk传递。如果它是一个函数，它只是等待函数处理并返回响应。如果它不是一个函数，它只是正常处理。
