##### Redux

Redux是JavaScript应用程序的可预测状态容器。Redux由Flux演变而来，但受Elm的启发，避开了Flux的复杂性。

Redux源文件由ES2015编写，但是会预编译到CommonJS和UMD规范的ES6，所以它可以支持任何现代浏览器。你不必非得使用Babel或模块打包器来使用Redux。

###### 要点

应用中所有的 state 都以一个对象树的形式储存在一个单一的 *store* 中。 惟一改变 state 的办法是触发 *action*，一个描述发生什么的对象。 为了描述 action 如何改变 state 树，你需要编写 *reducers*。

###### 动机

随着 JavaScript 单页应用开发日趋复杂，**JavaScript 需要管理比任何时候都要多的 state （状态）**。 这些 state 可能包括服务器响应、缓存数据、本地生成尚未持久化到服务器的数据，也包括 UI 状态，如激活的路由，被选中的标签，是否显示加载动效或者分页器等等。

**Redux 试图让 state 的变化变得可预测**。

###### 核心概念

要想更新 state 中的数据，你需要发起一个 action。Action 就是一个普通 JavaScript 对象（注意到没，这儿没有任何魔法？）用来描述发生了什么。

强制使用 action 来描述所有变化带来的好处是可以清晰地知道应用中到底发生了什么。如果一些东西改变了，就可以知道为什么变。action 就像是描述发生了什么的指示器。最终，为了把 action 和 state 串起来，开发一些函数，这就是 reducer。再次地，没有任何魔法，reducer 只是一个接收 state 和 action，并返回新的 state 的函数。

但是主要的想法是如何根据这些 action 对象来更新 state，而且 90% 的代码都是纯 JavaScript，没用 Redux、Redux API 和其它魔法。

###### 三大原则

单一数据源

**整个应用的 [state](https://www.redux.org.cn/docs/Glossary.html#state) 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 [store](https://www.redux.org.cn/docs/Glossary.html#store) 中。**

State是只读的

**唯一改变 state 的方法就是触发 [action](https://www.redux.org.cn/docs/Glossary.html#action)，action 是一个用于描述已发生事件的普通对象。**

使用纯函数来执行修改

**为了描述 action 如何改变 state tree ，你需要编写 [reducers](https://www.redux.org.cn/docs/Glossary.html#reducer)。**

Reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state

##### Action

**Action** 是把数据从应用（译者注：这里之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的**唯一**来源。一般来说你会通过 [`store.dispatch()`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 将 action 传到 store。

Action 本质上是 JavaScript 普通对象。我们约定，action 内必须使用一个字符串类型的 `type` 字段来表示将要执行的动作。多数情况下，`type` 会被定义成字符串常量。

除了 `type` 字段外，action 对象的结构完全由你自己决定。

Action创建函数

**Action 创建函数** 就是生成 action 的方法。

在 Redux 中的 action 创建函数只是简单的返回一个 action:

```
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

store 里能直接通过 [`store.dispatch()`](https://cn.redux.js.org/docs/api/Store.html#dispatch) 调用 `dispatch()` 方法，但是多数情况下你会使用 [react-redux](http://github.com/gaearon/react-redux) 提供的 `connect()` 帮助器来调用。[`bindActionCreators()`](https://cn.redux.js.org/docs/api/bindActionCreators.html) 可以自动把多个 action 创建函数 绑定到 `dispatch()` 方法上。

##### Reducer

**Reducers** 指定了应用状态的变化如何响应 [actions](https://www.redux.org.cn/docs/basics/Actions.html) 并发送到 store 的，记住 actions 只是描述了*有事情发生了*这一事实，并没有描述应用如何更新 state。

reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。(previousState, action) => newState

**永远不要**在 reducer 里做这些操作：

- 修改传入参数；
- 执行有副作用的操作，如 API 请求和路由跳转；
- 调用非纯函数，如 `Date.now()` 或 `Math.random()`。

**只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯执行计算。**

注意：

1. **不要修改 `state`。** 使用 [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 新建了一个副本。不能这样使用 `Object.assign(state, { visibilityFilter: action.filter })`，因为它会改变第一个参数的值。你**必须**把第一个参数设置为空对象。你也可以开启对ES7提案[对象展开运算符](https://www.redux.org.cn/docs/recipes/UsingObjectSpreadOperator.html)的支持, 从而使用 `{ ...state, ...newState }` 达到相同的目的。
2. **在 `default` 情况下返回旧的 `state`。**遇到未知的 action 时，一定要返回旧的 `state`。

[`combineReducers()`](https://www.redux.org.cn/docs/api/combineReducers.html) 所做的只是生成一个函数，这个函数来调用你的一系列 reducer，每个 reducer **根据它们的 key 来筛选出 state 中的一部分数据并处理**，然后这个生成的函数再将所有 reducer 的结果合并成一个大的对象。

**注意每个 reducer 只负责管理全局 state 中它负责的一部分。每个 reducer 的 `state` 参数都不同，分别对应它管理的那部分 state 数据。**

[`combineReducers()`](https://cn.redux.js.org/docs/api/combineReducers.html) 所做的只是生成一个函数，这个函数来调用你的一系列 reducer，每个 reducer **根据它们的 key 来筛选出 state 中的一部分数据并处理**，然后这个生成的函数再将所有 reducer 的结果合并成一个大的对象。[没有任何魔法。](https://github.com/gaearon/redux/issues/428#issuecomment-129223274)正如其他 reducers，如果 combineReducers() 中包含的所有 reducers 都没有更改 state，那么也就不会创建一个新的对象。

##### Store

在前面的章节中，我们学会了使用 [action](https://www.redux.org.cn/docs/basics/Actions.html) 来描述“发生了什么”，和使用 [reducers](https://www.redux.org.cn/docs/basics/Reducers.html) 来根据 action 更新 state 的用法。

**Store** 就是把它们联系到一起的对象。Store 有以下职责：

- 维持应用的 state；
- 提供 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 方法获取 state；
- 提供 [`dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 方法更新 state；
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 注册监听器;
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 返回的函数注销监听器。

再次强调一下 **Redux 应用只有一个单一的 store**。当需要拆分数据处理逻辑时，你应该使用 [reducer 组合](https://cn.redux.js.org/docs/basics/Reducers.html#splitting-reducers) 而不是创建多个 store。

##### 数据流

**严格的单向数据流**是 Redux 架构的设计核心。

这意味着应用中所有的数据都遵循相同的生命周期，这样可以让应用变得更加可预测且容易理解。同时也鼓励做数据范式化，这样可以避免使用多个且独立的无法相互引用的重复数据。

Redux 应用中数据的生命周期遵循下面 4 个步骤：

**1.调用** [`store.dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch)。

[Action](https://www.redux.org.cn/docs/basics/Actions.html) 就是一个描述“发生了什么”的普通对象。

**2.Redux store 调用传入的 reducer 函数。**

[Store](https://www.redux.org.cn/docs/basics/Store.html) 会把两个参数传入 [reducer](https://www.redux.org.cn/docs/basics/Reducers.html)： 当前的 state 树和 action。

**3.根 reducer 应该把多个子 reducer 输出合并成一个单一的 state 树。**

根 reducer 的结构完全由你决定。Redux 原生提供[`combineReducers()`](https://www.redux.org.cn/docs/api/combineReducers.html)辅助函数，来把根 reducer 拆分成多个函数，用于分别处理 state 树的一个分支。

**4.Redux store 保存了根 reducer 返回的完整 state 树。**

这个新的树就是应用的下一个 state！所有订阅 [`store.subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 的监听器都将被调用；监听器里可以调用 [`store.getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 获得当前 state。

现在，可以应用新的 state 来更新 UI。如果你使用了 [React Redux](https://github.com/gaearon/react-redux) 这类的绑定库，这时就应该调用 `component.setState(newState)` 来更新。

##### 搭配React

这里需要再强调一下：Redux 和 React 之间没有关系。Redux 支持 React、Angular、Ember、jQuery 甚至纯 JavaScript。

尽管如此，Redux 还是和 [React](http://facebook.github.io/react/) 和 [Deku](https://github.com/dekujs/deku) 这类库搭配起来用最好，因为这类库允许你以 state 函数的形式来描述界面，Redux 通过 action 的形式来发起 state 变化。

###### 容器组件（Smart/Container Components）和展示组件（Dumb/Presentational Components）

|                | 展示组件                   | 容器组件                           |
| -------------: | :------------------------- | :--------------------------------- |
|           作用 | 描述如何展现（骨架、样式） | 描述如何运行（数据获取、状态更新） |
| 直接使用 Redux | 否                         | 是                                 |
|       数据来源 | props                      | 监听 Redux state                   |
|       数据修改 | 从 props 调用回调函数      | 向 Redux 派发 actions              |
|       调用方式 | 手动                       | 通常由 React Redux 生成            |

##### 使用对象展开运算符（Object Spread Operator）

从不直接修改 state 是 Redux 的核心理念之一, 所以你会发现自己总是在使用 [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 创建对象拷贝, 而拷贝中会包含新创建或更新过的属性值。

一个可行的替代方案是使用 ES7 提案的 [对象展开运算符](https://github.com/sebmarkbage/ecmascript-rest-spread)。该提案让你可以通过展开运算符 (`...`) , 以更加简洁的形式将一个对象的可枚举属性拷贝至另一个对象。

compose(...functions) 从右到左来组合多个函数

###### 参数

1. (*arguments*): 需要合成的多个函数。预计每个函数都接收一个参数。它的返回值将作为一个参数提供给它左边的函数，以此类推。例外是最右边的参数可以接受多个参数，因为它将为由此产生的函数提供签名。（译者注：`compose(funcA, funcB, funcC)` 形象为 `compose(funcA(funcB(funcC())))`）

###### 返回值

(*Function*): 从右到左把接收到的函数合成后的最终函数。

##### 异步action创建函数

如何把 [之前定义](https://cn.redux.js.org/docs/advanced/AsyncActions.html#synchronous-action-creators) 的同步 action 创建函数和网络请求结合起来呢？标准的做法是使用 [Redux Thunk 中间件](https://github.com/gaearon/redux-thunk)。要引入 `redux-thunk` 这个专门的库才能使用。我们 [后面](https://cn.redux.js.org/docs/advanced/Middleware.html) 会介绍 middleware 大体上是如何工作的；目前，你只需要知道一个要点：通过使用指定的 middleware，action 创建函数除了返回 action 对象外还可以返回函数。这时，这个 action 创建函数就成为了 [thunk](https://en.wikipedia.org/wiki/Thunk)。

thunk 的一个优点是它的结果可以再次被 dispatch

[Thunk middleware](https://github.com/gaearon/redux-thunk) 并不是 Redux 处理异步 action 的唯一方式：

- 你可以使用 [redux-promise](https://github.com/acdlite/redux-promise) 或者 [redux-promise-middleware](https://github.com/pburtchaell/redux-promise-middleware) 来 dispatch Promise 来替代函数。
- 你可以使用 [redux-observable](https://github.com/redux-observable/redux-observable) 来 dispatch Observable。
- 你可以使用 [redux-saga](https://github.com/yelouafi/redux-saga/) 中间件来创建更加复杂的异步 action。
- 你可以使用 [redux-pack](https://github.com/lelandrichardson/redux-pack) 中间件 dispatch 基于 Promise 的异步 Action。
- 你甚至可以写一个自定义的 middleware 来描述 API 请求，就像这个 [真实场景的案例](https://cn.redux.js.org/docs/introduction/Examples.html#real-world) 中的做法一样。

##### 异步数据流

默认情况下，[`createStore()`](https://cn.redux.js.org/docs/api/createStore.html) 所创建的 Redux store 没有使用 [middleware](https://cn.redux.js.org/docs/advanced/Middleware.html)，所以只支持 [同步数据流](https://cn.redux.js.org/docs/basics/DataFlow.html)。

你可以使用 [`applyMiddleware()`](https://cn.redux.js.org/docs/api/applyMiddleware.html) 来增强 [`createStore()`](https://cn.redux.js.org/docs/api/createStore.html)。虽然这不是必须的，但是它可以帮助你[用简便的方式来描述异步的 action](https://cn.redux.js.org/docs/advanced/AsyncActions.html)。

##### Middleware

Redux middleware 被用于解决不同的问题，但其中的概念是类似的。**它提供的是位于 action 被发起之后，到达 reducer 之前的扩展点。** 你可以利用 Redux middleware 来进行日志记录、创建崩溃报告、调用异步接口或者路由等等。

##### 应该使用什么样的异步中间件? 怎样在 thunk、saga、observable 或其他类似的库中做出选择?

这里有 [许多可用的异步/副作用中间件](https://github.com/markerikson/redux-ecosystem-links/blob/master/side-effects.md), 但最常用的有 [`redux-thunk`](https://github.com/reduxjs/redux-thunk)、 [`redux-saga`](https://github.com/redux-saga/redux-saga)和 [`redux-observable`](https://github.com/redux-observable/redux-observable)。 它们是优势、弱点及用例各不相同的工具。

一般的经验是:

- Thunk 最适合复杂的同步逻辑（特别是需要访问整个 Redux 存储状态的代码）和简单的异步逻辑（如基本的 AJAX 调用）。 通过使用`async / await`，也可将 thunk 用于更复杂的基于 promise 的逻辑。
- Saga 最适合复杂的异步逻辑和解耦的“后台线程”类型的行为，特别是如果需要监听发起的（dispatched）action（这是通过 thunk 无法完成的事情）。 saga 需要使用者熟练使用 ES6 generator 函数和`redux-saga`的“effects”运算符。
- Observable 与 saga 解决了同一类问题，但依赖于 RxJS 来实现异步行为。 Observable 需要使用者熟练使用 RxJS API。

我们建议大多数 Redux 用户应该从 thunk 开始，如果他们的应用确实需要处理更复杂的异步逻辑，再添加一个额外的副作用库，如 saga 或 observable。

因为 saga 和 observable 具有相同的用例，所以应用程序通常使用其中一个，但不能同时使用两者。 但是，请注意 **同时使用 thunk 和 saga 或者 observable 是完全没问题的**，因为它们解决的是不同的问题。

##### 为什么 Redux 需要不变性？

- Redux 和 React-Redux 都使用了

  浅比较

  。具体来说：

  - Redux 的 `combineReducers` 方法 [浅比较](https://cn.redux.js.org/docs/faq/ImmutableData.html#how-redux-uses-shallow-checking) 它调用的 reducer 的引用是否发生变化。
  - React-Redux 的 `connect` 方法生成的组件通过 [浅比较根 state 的引用变化](https://cn.redux.js.org/docs/faq/ImmutableData.html#how-react-redux-uses-shallow-checking) 与 `mapStateToProps` 函数的返回值，来判断包装的组件是否需要重新渲染。 以上[浅比较需要不变性](https://cn.redux.js.org/docs/faq/ImmutableData.html#redux-shallow-checking-requires-immutability)才能正常工作

- 不可变数据的管理极大地提升了数据处理的安全性。

- 进行时间旅行调试要求 reducer 是一个没有副作用的纯函数，以此在不同 state 之间正确的移动。

##### 浅比较和深比较有何区别？

浅比较（也被称为 **引用相等**）只检查两个不同 **变量** 是否为同一对象的引用；与之相反，深比较（也被称为 **原值相等**）必须检查两个对象所有属性的 **值** 是否相等。

所以，浅比较就是简单的（且快速的）`a === b`，而深比较需要以递归的方式遍历两个对象的所有属性，在每一个循环中对比各个属性的值。

正是因为性能考虑，Redux 使用浅比较。

##### Redux 是如何使用浅比较的？

Redux 在 `combineReducers` 函数中使用浅比较来检查根 state 对象（root state object）是否发生变化，有修改时，返回经过修改的根 state 对象的拷贝，没有修改时，返回当前的根 state 对象。

##### React-Redux 是如何使用浅比较的？

React-Redux 使用浅比较来决定它包装的组件是否需要重新渲染。

首先 React-Redux 假设包装的组件是一个“纯”（pure）组件，即[给定相同的 props 和 state，这个组件会返回相同的结果](https://github.com/reactjs/react-redux/blob/f4d55840a14601c3a5bdc0c3d741fc5753e87f66/docs/troubleshooting.md#my-views-arent-updating-when-something-changes-outside-of-redux)。

做出这样的假设后，React-Redux 就只需检查根 state 对象或 `mapStateToProps` 的返回值是否改变。如果没变，包装的组件就无需重新渲染。

为了检测改变是否发生，React-Redux 会保留一个对根 state 对象的引用，还会保留 `mapStateToProps` 返回的 props 对象的**每个值**的引用。

最后 React-Redux 会对根 state 对象的引用与传递给它的 state 对象进行浅比较，还会对每个 props 对象的每个值的引用与 `mapStateToProps` 返回的那些值进行一系列浅比较。

##### 为什么 React-Redux 对 `mapStateToProps` 返回的 props 对象的每个值进行浅比较？

对 props 对象来说，React-Redux 会对其中的每个**值**进行浅比较，而不是 props 对象本身。

它这样做的原因是：props 对象实际上是一组由属性名和其值（或用于取值或生成值的 selector 函数）的键值对组成的。

##### React-Redux 是如何使用浅比较来决定组件是否需要重新渲染的？

每次调用 React-Redux 提供的 `connect` 函数时，它储存的根 state 对象的引用，与当前传递给 store 的根 state 对象之间，会进行浅比较。如果相等，说明根 state 对象没有变化，也就无需重新渲染组件，甚至无需调用 `mapStateToProps`。

如果发现其不相等，说明根 state 对象**已经**被更新了，这时 `connect` 会调用 `mapStateToProps` 来查看传给包装的组件的 props 是否被更新。

它会对该对象的每一个值各自进行浅比较，如果发现其中有不相等的才会触发重新渲染。
