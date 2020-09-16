##### Vue

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。

###### 声明式渲染

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统。现在数据和 DOM 已经被建立了关联，所有东西都是**响应式的**。

`v-bind` attribute 被称为**指令**。指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊 attribute。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。

`v-for` 指令可以绑定数组的数据来渲染一个项目列表

###### 处理用户输入

为了让用户和你的应用进行交互，我们可以用 `v-on` 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法。

Vue 还提供了 `v-model` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。

###### 数据与方法

当一个 Vue 实例被创建时，它将 `data` 对象中的所有的 property 加入到 Vue 的**响应式系统**中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

###### 实例生命周期钩子

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

###### 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值

```
<span>Message: {{ msg }}</span>
```

Mustache 语法不能作用在 HTML attribute 上，遇到这种情况应该使用 [`v-bind` 指令](https://cn.vuejs.org/v2/api/#v-bind)

```
<div v-bind:id="dynamicId"></div>
```

###### 指令

指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。指令 attribute 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute：

```
<a v-bind:href="url">...</a>
```

在这里href是参数，告知v-bind指令将该元素的href attribute与表达式url的值绑定

###### 修饰符

修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

```
<form v-on:submit.prevent='onSubmit'>...</form>
```

###### 缩写

v-bind缩写

```
<a v-bind:href='url'>...</a>
<a :href='url'>...</a>
动态参数的缩写
<a :[key]='url'>...</a>
```

v-on缩写

```
<a v-on:click='dosomething'></a>
<a @click='dosomething'></a>
动态参数的缩写
<a @[event]='dosomething'></a>
```

###### 计算属性

对于任何复杂逻辑，你都应当使用计算属性。computed

###### 计算属性缓存VS方法

可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

###### 条件渲染

`v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

`v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。

`v-else-if`，顾名思义，充当 `v-if` 的“else-if 块”，可以连续使用

另一个用于根据条件展示元素的选项是 `v-show` 指令。

不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS property `display`。

###### v-if vs v-show

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

###### 用key管理可复用的元素

###### v-if 与v-for一起使用

不推荐同时使用v-if 和v-for。当v-if与v-for一起使用时，v-for具有比v-if更高的优先级。这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中。

###### v-for

```
<li v-for="(item,index) in items" v-bind:key='index'></li>
```

为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key` attribute

###### 事件处理

可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。在 `methods` 对象中定义方法

###### 为什么在HTML中监听事件？

你可能注意到这种事件监听的方式违背了关注点分离 (separation of concern) 这个长期以来的优良传统。但不必担心，因为所有的 Vue.js 事件处理方法和表达式都严格绑定在当前视图的 ViewModel 上，它不会导致任何维护上的困难。实际上，使用 `v-on` 有几个好处：

1. 扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法。
2. 因为你无须在 JavaScript 里手动绑定事件，你的 ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试。
3. 当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除。你无须担心如何清理它们。

###### 表单输入绑定

你可以用 `v-model` 指令在表单 <input>、<textarea> 及 <select>元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。

##### 组件基础

###### data必须是一个函数

一个组件的data选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝。

```
data:function(){
	return {
		count: 0
	}
}
```

###### 组件的组织

组件的注册类型：**全局注册**和**局部注册**。至此，我们的组件都只是通过 `Vue.component` 全局注册的。

全局注册的组件可以用在其被注册之后的任何 (通过 `new Vue`) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。

###### 通过prop向子组件传递数据

###### 单个根元素 每个组件必须只有一个根元素

###### 单向数据流

所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外变更父级组件的状态，从而导致你的应用的数据流向难以理解。

额外的，每次父级组件发生变更时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你**不**应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。

父组件向子组件传递数据，可以通过prop属性向下传递数据，子组件向父组件传递数据是通过事件给父组件发送消息。

非父子组件之间传值，需要定义个公共的实列文件来作为中间仓库来传值的。比如定义一个叫 bus.js 文件。
所谓中间仓库就是创建一个事件中心，相当于中转站，可以使用它来传递事件和接收事件的。

```
import Vue from 'vue';
import VueRouter from 'vue-router';

// 告诉 vue 使用 vueRouter
Vue.use(VueRouter);
```

###### VUE字符串模板和非字符串模板的区别

字符串模板：指的是在组件选项里用template:''指定的模板，换句话说，写在js中的template:''中的就是字符串模板。比如下面这个：

```
var tmp= new Vue({
	template:''
})
```

非字符串模板：在单文件里用指定的模板，换句话说，写在html中的就是非字符串模板。

HTML 模板：应该指的是原生HTML，通过 `el` 挂载到 Vue 实例上。

首先，Vue 会将 template 中的内容插到 DOM 中，以方便解析标签。由于 HTML 标签不区分大小写，所以在生成的标签名都会转换为小写。例如，当你的 template 为 <MyComponent></MyComponent> 时，插入 DOM 后会被转换为 <mycomponent></mycomponent>。

然后，通过标签名寻找对应的自定义组件。**匹配的优先顺序从高到低为：原标签名、camelCase化的标签名、PascalCase化的标签名。**例如 <my-component> 会依次匹配 my-component、myComponent、MyComponent。



###### 注意：组件名大小写

注意：当直接在DOM中使用一个组件（而不是字符串模板或单文件组件）的时候，我们强烈推荐遵循W3C规范中的自定义组件名（字母全小写且必须包含一个连字符）。这会帮助你避免和当前以及未来的HTML元素相冲突。

（1）使用kebab-case:

```
Vue.component('my-component-name',{});
```

当使用kebab-case(短横线分隔命名)定义一个组件时，你也必须在引用这个自定义元素时使用kebad-case，例如<my-component-case>。

（2）使用PascalCase:

```
Vue.component('MyComponentName',{})
```

当使用PascalCase(驼峰式命名)定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说<my-component-name>和<MyComponentName>都是可以接受的。注意，尽管如此，直接在DOM（即非字符串的模板，如在单个组件的<template></template>中或者index.html中直接CDN引入vue.js的<div id='app'></div>，使用驼峰式命名或短横线分隔命名都可以）使用时只有kebad-case是有效的，使用驼峰式，是不会渲染的。



**1. 什么是vuex？**

Vuex官网的解释：Vuex是一个专为vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

与其他模式不同的是，Vuex 是专门为 Vue.js 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同：

1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

##### state 单一状态树

Vuex 使用**单一状态树**——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 ([SSOT](https://en.wikipedia.org/wiki/Single_source_of_truth))”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

######  在 Vue 组件中获得 Vuex 状态

由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在[计算属性](https://cn.vuejs.org/guide/computed.html)中返回某个状态。

Vuex通过store选项，提供了一种机制将状态从根组件“注入”到每一个子组件中（需要调用Vue.use(vuex)）

```
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

通过在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 `this.$store` 访问到。

```
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

###### `mapState` 辅助函数

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键：

######  对象展开运算符

`mapState` 函数返回的是一个对象。我们如何将它与局部计算属性混合使用呢？通常，我们需要使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 `computed` 属性。但是自从有了[对象展开运算符](https://github.com/tc39/proposal-object-rest-spread)，我们可以极大地简化写法

```
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

###### 组件仍然保有局部状态

使用 Vuex 并不意味着你需要将**所有的**状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。

##### Getter

有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数：

```
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

Getter 接受 state 作为其第一个参数：

```
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
}
```

###### `mapGetters` 辅助函数

`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性

##### Mutation

更改Vuex的store中的状态的唯一方法是提交mutation。Vuex中的mutation非常类似于事件：每个mutation都有一个字符串的事件类型（type）和一个回调函数（handler）。这个回调函数就是我们实际进行状态更改的地方，并且它会接受state作为第一个参数。

```
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

###### Mutation必须是同步函数

###### Mutation需遵守Vue的响应规则

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该

- 使用 `Vue.set(obj, 'newProp', 123)`, 或者

- 以新对象替换老对象。例如，利用[对象展开运算符](https://github.com/tc39/proposal-object-rest-spread)我们可以这样写：

  ```js
  state.obj = { ...state.obj, newProp: 123 }
  ```

在 Vuex 中，**mutation 都是同步事务**

##### Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。当我们在之后介绍到 [Modules](https://vuex.vuejs.org/zh/guide/modules.html) 时，你就知道 context 对象为什么不是 store 实例本身了。

实践中，我们会经常用到 ES2015 的 [参数解构](https://github.com/lukehoban/es6features#destructuring) 来简化代码（特别是我们需要调用 `commit` 很多次的时候）：

```
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

###### 分发Action

Action通过store.dispatch方法触发：

```
store.dispatch('increment');
```

Actions支持同样的载荷方式和对象方式进行分发：

```
//以载荷形式分发
store.dispatch('incrementAsync',{
	amount:10
})
//以对象形式分发
store.dispatch({
	type:'incrementAsync',
	amount:10
})
```

###### 在组件中分发Action

你在组件中使用this.$store.dispatch('xxx')分发action，或者使用mapActions辅助函数将组件的methods映射为store.dispatch调用（需要先在根节点注入store）

```
import {mapActions} from 'vuex'

export default{
//...
 methods: {
 	...mapActions([
 		'increment', //将this.increment()映射为this.$store.dispatch('increment')
 		//mapActions也支持载荷
 		'incrementBy' 
 		//将this.incrementBy(amount)映射为this.$store.dispatch('incrementBy',amount)
 	]),
 	...mapActions({
 		add: 'increment' //将this.add()映射为this.$store.dispatch('increment')
 	})
 }
}
```

###### 组合Action

首先，你需要明白 store.dispatch 可以处理被触发的action的处理函数返回的Promise，并且store.dispatch仍旧返回Promise。

最后，利用async/await ，可以如下组合action

```
actions: {
	async actionA({commit}){
		commit('getData', await getData())
	},
	async actionB({dispatch, commit}){
		await dispatch(actionA) //等待actionA完成
		commit('getOtherData', await getOtherData())
	}
}
```

一个store.dispatch在不同模块中可以触发多个action函数。在这种情况下，只有当所有触发函数完成后，返回的Promise才会执行。

##### Module

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割

###### 命名空间

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。

启用了命名空间的 getter 和 action 会收到局部化的 `getter`，`dispatch` 和 `commit`。换言之，你在使用模块内容（module assets）时不需要在同一模块内额外添加空间名前缀。更改 `namespaced` 属性后不需要修改模块内的代码。

Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 **action** 里面。

##### 严格模式

开启严格模式，仅需在创建 store 的时候传入 `strict: true`

在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。

**不要在发布环境下启用严格模式**！



官网的解释很模糊。其实在vue组件开发中，经常会需要将当前的组件的数据传递给其他的组件，父子组件通信的话，我们可以采用props+emit 这种方式，如上面的demo方式来传递数据，但是当通信双方不是父子组件甚至根本不存在任何关系的时候，或者说一个状态需要共享给多个组件的时候，那么就会非常麻烦，数据维护也相当的不好维护，因此就出现了vuex。它能帮助我们把公用的状态抽出来放在vuex的容器中，然后根据一定的规则进行管理。vuex采用了集中式存储管理所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

```
<script type="text/javascript">
    Vue.use(Vuex); // 使用vuex
    var myStore = new Vuex.Store({
      // state是存储状态 定义应用状态全局的数据结构
      state: {
        name: 'kongzhi',
        todoLists: []
      },
      /*
        mutations是提交状态修改，也就是set、get中的set，这是vuex中唯一修改state的方式，但是不支持异步操作。
        每个mutation都有一个字符串的事件类型(type)和一个回调函数(handler)
        第一个参数默认是state，外部调用方式为：store.commit('SET_AGE', 30).
      */
      mutations: {
        // 新增list
        ADDLIST(state, item) {
          state.todoLists.push(item);
        },
        // 删除list中的项
        DELLIST(state, index) {
          state.todoLists.splice(index, 1);
        },
        // 设置 错误提示信息
        SETERROR(state, msg) {
          state.message = msg;
        }
      },
      /*
        getters是从state中派生出状态的。也就是set、get中的get，它有两个可选的参数，state和getters，
        分别可以获取state中的变量和其他的getters。外部调用的方式：store.getters.todoCount()
      */
      getters: {
        todoCount(state) {
          return state.todoLists.length;
        }
      },
      /*
       和上面的mutations类似，但是actions支持异步操作的，外部调用方式为：store.dispatch('nameAction')
       常见的使用是：从服务器端获取数据，在数据获取完成后会调用 store.commit()来更改Store中的状态。
       Action函数接收一个与store实列具有相同方法和属性的context对象，因此我们可以使用 context.commit 提交一个
       mutation，或者通过 context.state 和 context.getters来获取state和getters
      */
      actions: {
        addList(context, item) {
          if (item) {
            context.commit('ADDLIST', item);
            context.commit('SETERROR', '');
          } else {
            context.commit('SETERROR', '添加失败');
          }
        },
        delList(context, index) {
          context.commit('DELLIST', index);
          context.commit('SETERROR', '删除成功');
        }
      },
      /*
       modules 对象允许将单一的Store拆分为多个Store的同时保存在单一的状态树中。
      */
      modules: {

      }
    });
    new Vue({
      el: '#app',
      data: {
        name: 'init name'
      },
      store: myStore,
      mounted: function() {
        console.log(this);
      }
    })
  </script>
</body>
</html>
```

new Vuex.store({}) 含义是创建一个Vuex实列。store是vuex的一个核心方法，字面含义为 '仓库'的意思。实列化完成后，需要注入到vue实列中，它有五个核心的选项，state、mutations、getters、actions和modules。

 **vuex中如何获取state的数据呢？**

在组件内部中的computed来获取state的数据(computed是实时响应的)。

**如何对state的数据进行筛选和过滤**

有时候，我们需要对state的数据进行刷选和过滤操作，比如后台请求回来的数据，我们需要进行数据过滤操作，getters就可以了。

 **mutations操作来改变state数据**

如上是如何获取state的数据了，那么现在我们使用mutations来改变state的数据了，在Vuex中，改变状态state的唯一方式是通过提交commit的一个mutations，mutations下的对应函数接收第一个参数state，第二个参数为payload(载荷)，payload一般是一个对象，用来记录开发时使用该函数的一些信息。mutations是处理同步的请求。不能处理异步请求的。

**actions操作mutations异步来改变state数据**
actions可以异步操作，actions提交mutations，通过mutations来提交数据的变更。它是异步修改state的状态的。
外部调用方式是 this.$store.dispatch('nameAsyn');

**理解context:** context是和 this.$store 具有相同的方法和属性的对象。我们可以通过 context.state 和 context.getters来获取state和getters。

**理解dispatch:** 它含有异步操作，含义可以理解为 '派发'，比如向后台提交数据，可以为 this.$store.dispatch('actions方法名', 值);

![image-20200616150049837](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20200616150049837.png)

组件派发任务到actions，actions触发mutations中的方法，然后mutations来改变state中的数据，数据变更后响应推送给组件，组件重新渲染。

```
export default {
     namespaced:true,//用于在全局引用此文件里的方法时标识这一个的文件名
     state,
     getters,
     mutations,
     actions
}
```

```
computed:{
    ...mapState({  //这里的...是超引用，ES6的语法，意思是state里有多少属性值我可以在这里放多少属性值
         isShow:state=>state.footerStatus.showFooter 
         //注意这些与上面的区别就是state.footerStatus,
         //里面定义的showFooter是指footerStatus.js里state的showFooter
      }),
methods:{
      ...mapActions('collection',[ //collection是指modules文件夹下的collection.js
          'invokePushItems'  
          //collection.js文件中的actions里的方法，在上面的@click中执行并传入实参
      ])
  }
```

//mand-mobile 、vue-router 、vuex

##### Vue Router

Vue Router 是 [Vue.js](http://cn.vuejs.org/) 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

当你要把 Vue Router 添加进来，我们需要做的是，将组件 (components) 映射到路由 (routes)，然后告诉 Vue Router 在哪里渲染它们。

##### 动态路由匹配

动态路径参数

一个“路径参数”使用冒号 `:` 标记。当匹配到一个路由时，参数值会被设置到 `this.$route.params`，可以在每个组件内使用。

##### 嵌套路由

需要在 `VueRouter` 的参数中使用 `children` 配置

**要注意，以 `/` 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。**

##### 编程式的导航

**注意：在 Vue 实例内部，你可以通过 `$router` 访问路由实例。因此你可以调用 `this.$router.push`。**

想要导航到不同的 URL，则使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

当你点击 <route-link>`` 时，这个方法会在内部调用，所以说，点击 `` <router-link :to="...">等同于调用 `router.push(...)`。

| 声明式                  | 编程式           |
| ----------------------- | ---------------- |
| <router-link :to="..."> | router.push(...) |

###### `router.replace(location, onComplete?, onAbort?)`

跟 `router.push` 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

| 声明式                          | 编程式                |
| ------------------------------- | --------------------- |
| <router-link :to="..." replace> | `router.replace(...)` |

###### [#](https://router.vuejs.org/zh/guide/essentials/navigation.html#router-go-n)`router.go(n)`

这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。

例子

```js
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```

###### 操作 History

你也许注意到 `router.push`、 `router.replace` 和 `router.go` 跟 [`window.history.pushState`、 `window.history.replaceState` 和 `window.history.go`](https://developer.mozilla.org/en-US/docs/Web/API/History)好像， 实际上它们确实是效仿 `window.history` API 的。

##### 命名路由

有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。你可以在创建 Router 实例的时候，在 `routes` 配置中给某个路由设置名称。

```
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

要链接到一个命名路由，可以给 `router-link` 的 `to` 属性传一个对象

```
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

##### 命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar` (侧导航) 和 `main` (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 `router-view` 没有设置名字，那么默认为 `default`。

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 s)：

```
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

##### 重定向和别名

重定向也是通过routes配置来完成。

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```

重定向的目标也可以是一个命名的路由

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
```

甚至 是一个方法，动态返回重定向目标

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```

###### 别名

“重定向”的意思是，当用户访问 `/a`时，URL 将会被替换成 `/b`，然后匹配路由为 `/b`，那么“别名”又是什么呢？

**`/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。**

```
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。

##### 路由组件传参

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

使用 `props` 将组件和路由解耦。

##### HTML5 History 模式

`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式充分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面。

当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，也好看！

不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404，这就不好看了。

所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 app 依赖的页面。

###### 区别：

最直观的区别就是在url中 hash 带了一个很丑的 # 而history是没有#的

hash —— 即地址栏 URL 中的 # 符号（此 hash 不是密码学里的散列运算）。比如这个 URL：http://www.abc.com/#/hello hash 的值为 #/hello。它的特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。
history —— 利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持）这两个方法应用于浏览器的历史记录栈，在当前已有的 back、forward、go 的基础之上，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的 URL，但浏览器不会立即向后端发送请求。
因此可以说，hash 模式和 history 模式都属于浏览器自身的特性，Vue-Router 只是利用了这两个特性（通过调用浏览器提供的接口）来实现前端路由.

##### 导航守卫

正如其名，`vue-router` 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。

记住**参数或查询的改变并不会触发进入/离开的导航守卫**。你可以通过[观察 `$route` 对象](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#响应路由参数的变化)来应对这些变化，或使用 `beforeRouteUpdate` 的组件内守卫。

###### 全局前置守卫

使用 `router.beforeEach` 注册一个全局前置守卫。

###### 全局解析守卫

在 2.5.0+ 你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

###### 全局后置钩子

你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身

###### 路由独享的守卫

你可以在路由配置上直接定义 `beforeEnter` 守卫

###### 组件内的守卫

###### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。

###### 路由元信息

定义路由的时候可以配置 `meta` 字段

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

那么如何访问这个 `meta` 字段呢？

首先，我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录

例如，根据上面的路由配置，`/foo/bar` 这个 URL 将会匹配父路由记录以及子路由记录。

一个路由匹配到的所有路由记录会暴露为 `$route` 对象 (还有在导航守卫中的路由对象) 的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 `meta` 字段。

##### 过渡动效

`` <router-view>是基本的动态组件，所以我们可以用 `` <transition>组件给它添加一些过渡效果。

```
<transition>
  <router-view></router-view>
</transition>
```

###### 单个路由的过渡

上面的用法会给所有路由设置一样的过渡效果，如果你想让每个路由组件有各自的过渡效果，可以在各路由组件内使用 <transition> 并设置不同的 name。

```
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}
```

##### 数据获取

有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

- **导航完成之后获取**：先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
- **导航完成之前获取**：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

从技术角度讲，两种方式都不错 —— 就看你想要的用户体验是哪种。

##### 滚动行为

使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 `vue-router` 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

**注意: 这个功能只在支持 `history.pushState` 的浏览器中可用。**

当创建一个 Router 实例，你可以提供一个 `scrollBehavior` 方法：

```
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

`scrollBehavior` 方法接收 `to` 和 `from` 路由对象。第三个参数 `savedPosition` 当且仅当 `popstate` 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

这个方法返回滚动位置的对象信息，长这样：

- `{ x: number, y: number }`
- `{ selector: string, offset? : { x: number, y: number }}` (offset 只在 2.6.0+ 支持)

如果返回一个 falsy (译者注：falsy 不是 `false`，[参考这里](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy))的值，或者是一个空对象，那么不会发生滚动。

##### 路由懒加载

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

结合 Vue 的[异步组件](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)和 Webpack 的[代码分割功能](https://doc.webpack-china.org/guides/code-splitting-async/#require-ensure-/)，轻松实现路由组件的懒加载。

首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：

```
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })
```

第二，在 Webpack 2 中，我们可以使用[动态 import](https://github.com/tc39/proposal-dynamic-import)语法来定义代码分块点 (split point)：

```
import('./Foo.vue') // 返回 Promise
```

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```
const Foo = () => import('./Foo.vue')
```

在路由配置中什么都不需要改变，只需要像往常一样使用 `Foo`：

```
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

###### 把组件按组分块

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 [命名 chunk](https://webpack.js.org/guides/code-splitting-require/#chunkname)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

