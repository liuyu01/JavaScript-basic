vue记录

template的作用是模板占位符，可帮助我们包裹元素，但在循环过程中，template不会被渲染到页面上。

template标签是不会渲染成标签元素的，因此里面必须要有一个父级标签，通常直接放个div即可

一个空vue.js的单文件组件是这样的：

```
<template>
	<div>
	</div>
</template>

<script>
export default{
	data(){
		return {}
	}
}

</script>
```

##### vue组件使用分三步：

1.引用组件 

```
import facePop from './components/facePop'
```

2.注册组件

```
components = {facePop}
```

3.使用组件

```
<facePop></facePop>
```

新建一个components文件存放组件

src/components/facePop.vue

```
<template>
	<div>
		<h2>我是一个facePop组件</h2>
	</div>
</template>
```

src/index.vue

```
<template>
	<div>
		<facePop></facePop>  //3.在模板中使用kebab-case、PascalCase两种都可以
		<face-pop></face-pop>
	</div>
</template>

<script>
	import facePop from './components/facePop';//1.引入组件 import后的名字一般与组件名称相同，也可不一样
	export default{
		data(){},
		components:{
			facePop  //2.注册组件 一般直接取一个名字 即 facePop: facePop
		}
	}
</script>
```

##### 组件名大小写

定义组件名的方式有两种：

（一）使用kebab-case 

```
Vue.component('my-component-name',{...})
```

当使用kebab-case(短横线分隔命名)定义一个组件时，你也必须在引用这个自定义元素时使用kebab-case，例如<my-component-name>。

（二）使用PascalCase

```
Vue.component('MyComponentName',{...})
```

当使用PascalCase(首字母大写命名)定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说<my-component-name>和<MyComponentName>都是可接受的。注意，尽管如此，直接在DOM（非字符串模板）中使用时只有kebab-case是有效的。

我们使用字符串模板，所以这个限制就不存在了。

##### 组件之间传值

在Vue中，父子组件的关系可以总结为**prop**向下传递，**事件**向上传递。父组件通过**prop**给子组件下发数据，子组件通过**事件**给父组件发送消息。父组件可以使用 props 把数据传给子组件、子组件可以使用 $emit 触发父组件的自定义事件。

子组件直接通过this.$emit("自定义事件")，然后父组件在组件中添加@自定义事件=‘event’就可以实现了。

vm.$emit( event, arg ) //触发当前实例上的事件

vm.$on( event, fn );//监听event事件后运行 fn； 

##### 单向数据流

prop是单向绑定的：当父组件的属性变化时，将传给子组件，但是反过来不会。这是为了防止子组件无意间修改了父组件的状态，来避免应用的数据流变得难以理解。另外，每次父组件更新时，子组件的所有prop都会更新为最新值。这意味着你不应该在子组件内部改变prop。如果你这么做了，Vue会在控制台给出警告。

注意在JavaScript中对象和数组是引用类型，指向同一个内存空间，如果prop是一个对象或数组，在子组件内部改变它会影响父组件的状态。

##### $refs的使用场景

父组件调用子组件的方法，可以传递数据
**注意**：子组件标签中的时间也不区分大小写要用“-”隔开

父组件：

```
<template>
  <div id="app">
    <child-a ref="child"></child-a>
    <!--用ref给子组件起个名字-->
    <button @click="getMyEvent">点击父组件</button>
  </div>
</template>
<script>
  import ChildA from './components/child.vue'
  export default {
    components: {
      ChildA
    },
    data() {
      return {
        msg: "我是父组件中的数据"
      }
    },
    methods: {
      getMyEvent(){
          this.$refs.child.emitEvent(this.msg);
          //调用子组件的方法，child是上边ref起的名字，emitEvent是子组件的方法。
      }
    }
  }
</script>
```

子组件：

```
<template>
  <button>点击我</button>
</template>
<script>
  export default {
    methods: {
      emitEvent(msg){
        console.log('接收的数据--------->'+msg)//接收的数据--------->我是父组件中的数据
      }
    }
  }
</script>
```

##### v-bind缩写

```
<!--完整语法-->
<a v-bind:href='url'>测试</a>
<!--缩写-->
<a :href='url'>测试</a>
```

##### v-on缩写

```
<!--完整语法-->
<a v-on:click='doSomething'>修改</a>
<!--缩写-->
<a @click='doSomething'>修改</a>
```

v-bind指令用于设置HTML属性。v-on指令用于绑定HTML事件。

Vue.js的指令是指v-开头，作用于html标签，提供一些特殊的特性，当指令被绑定到html元素的时候，指令会为被绑定的元素添加一些特殊的行为，可以将指令看成html的一种属性。

内置指令：

v-text  数据绑定标签，将vue对象data中的属性绑定给对应的标签作为内容显示出来，类似js的text属性;

v-html 类似v-text标签，他是将data的属性作为html语法输出，类似js中的innerHtml属性;

v-show  根据表达式的真假来切换所绑定的dom的display属性  v-show用于控制dom的显隐，实际是控制了dom的css的display属性

v-if 可以实现条件渲染，Vue会根据表达式的值的真假条件来渲染元素。

v-if和v-show功能差不多，都是用来控制dom的显隐，用法也一样，只是原理不同，当v-if=‘false’时，dom被直接删除掉；为true时，又重新渲染此dom;

注意：v-if有更高的切换开销。v-show有更高的初始渲染开销。因此，如果要非常频繁的切换，则使用v-show较好；如果在运行时条件不太可能改变，则v-if较好。

v-for 根据遍历数组来进行渲染

注意：当v-for和v-if同处于一个节点时，v-for的优先级比v-if更高。这意味着v-if将运行在每个v-for循环中。

v-on 缩写 @ 参数： event  主要用来监听dom事件，以便执行一些代码块。表达式可以是一个方法名。

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用`v-on:click.prevent.self`会**阻止所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

v-bind 缩写： 参数：attr/Prop 用于将vue的值绑定给对应dom的属性值。用来**动态的绑定一个或者多个特性**。没有参数时，可以绑定到一个包含键值对的对象。常用于动态绑定class和style。以及href等。

v-model 这个指令用于在表单上创建双向数据绑定。v-model会忽略所有表单元素的value、checked、selected特性的初始值。因为它选择Vue实例数据作为具体的值。

v-model修饰符：

v-mode.lazy: 一般v-model绑定的数据都是实时同步的，加上这个修饰符可以等到change事件再同步另一个的值；

v-model.number：自动将输入的值转为数值类型

v-model.trim:自动trim

v-slot 缩写 # 参数：插槽名（默认default）

##### data必须是函数

```
data(){
	return {};
}
```

一个组件的data选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝。

注意当点击按钮时，每个组件都会各自独立维护它的 `count`。因为你每用一次组件，就会有一个它的新**实例**被创建。

##### 模板语法

Vue.js使用了基于HTML的模板语法，允许开发者声明式地将DOM绑定至底层Vue实例的数据。所有vue.js的模板都是合法的HTML，所以能被遵循规范的浏览器和HTML解析器解析。

数据绑定最常见的形式就是使用“Mustache”语法（双大括号）的文本插值。

```
<span>Message:{{msg}}</span>
```

##### 计算属性 computed

```
computed: {
	...mapState({
		myCardMysInfo: state => state.mine.myCardMysInfo
	})
}
```

//computed 先通过... 之后通过mapState这个方法，把state中的数据取出来。这个state指store中初始化的state,当进行server请求后，把数据放到state中存起来。

##### 计算属性缓存VS方法

可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的响应式依赖进行缓存的。只在相关响应依赖发生改变时它们才会重新求值。这就意味着只要message还没有发生改变，多次访问reversedMessage计算属性会立即返回之前的计算结果，而不必再次执行函数。

##### methods

methods 对象定义当前页面操作的方法

```
methods:{
	//获取卡密弹框
    getCardMysInfo(){
    	this.getCardMystery({
    		body:{applicationId: this.applicationInfoId}, //参数
    		callback: data=>{ //回调
    			
    		}
    	})
    },
    hidePopUpCenter(){
    
    },
    ,,,
    ...mapActions("mine",['getCardMystery']); 
    // 此方法指明，this.getCardMystery是定义在mine(store)下的方法
}
```

##### 生命周期钩子函数

new Vue() -> beforeCreate() -> created() -> beforeMount() -> mounted() -> beforeUpdate -> updated() -> 

beforeDestory() -> destoryed() 

其余的方法：created(){} 创建的时候执行、mounted(){}挂载完成的时候执行、destroyed(){}销毁的时候执行、就是生命周期钩子函数。beforeRouteEnter(to, from, next){} 在路由进入后执行（路由守卫）

##### Vuex

Vuex官网的解释：Vuex是一个专为vue.js应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生改变。

与其他模式不同的是，Vuex是专门为Vue.js设计的状态管理库，以利用Vue.js的细粒度数据响应机制来进行高效的状态更新。

每一个Vuex应用的核心就是store(仓库)。“store”基本上就是一个容器，它包含着你的应用中大部分的状态（state）。Vuex和单纯的全局对象有以下两点不同：

1.Vuex的状态存储是响应式的。当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会相应地得到高效更新。

2.你不能直接改变store中的状态。改变store中的状态的唯一途径就是显式地提交(commit)mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

##### Mutation

更改Vuex的store中的状态的唯一方法是提交mutation。Vuex中的mutation非常类似于事件：每个mutation都有一个字符串的事件类型(type)和一个回调函数(handler)。这个回调函数就是我们实际进行状态更改的地方，并且它会接受state作为第一个参数。

```
const mutations = {
	MY_GAME_URL:(state, payload)=>{
		state.myGameUrl = payload;
	}
};
```

##### Mutation必须是同步函数

##### Mutation需遵守Vue的响应规则

既然Vuex的store中的状态是响应式的，那么当我们变更状态时，监视状态的Vue组件也会自动更新。这也意味着Vuex中的mutation也需要与使用Vue一样遵守一些注意事项：

1.最好提前在你的store中初始化好所有所需属性。

2.当需要在对象上添加新属性时，你应该使用Vue.set(obj, 'newProp', 123)，或者以新对象替换老对象。例如，利用对象展开运算符我们可以这样写：state.obj = {...state.obj, newProp: 123}

在Vuex中，mutation都是同步事务

##### Action

Action类似于mutation，不同在于：

- Action提交的是mutation，而不是直接变更状态
- Action可以包含任意异步操作

Action函数接受一个与store实例具有相同方法和属性的context对象，因此你可以调用context.commit提交一个mutation，或者通过context.state和context.getters来获取state和getters。

```
const actions = {
	getMyGameUrlData: async({commit},payload)=>{
		try{
			const {body, callback}=payload;
			const {data=''} = await getMyGameUrl(body);
			commit('MY_GAME_URL', data);
			callback && callback(data);
		}catch(error){
		
		}
	}
};
```

##### 分发Action

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

##### 在组件中分发Action

在组件中使用this.$store.dispatch('xxx')分发action，或者使用mapActions辅助函数将组件的methods映射为store.dispatch调用（需要先在根节点注入store）

##### 组合Action

首先，你需要明白store.dispatch可以处理被触发的action的处理函数返回的Promise，并且store.dispatch仍旧返回Promise。

最后，利用async/await，可以组合action

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

Vuex并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

1.应用层级的状态应该集中到单个store对象中。

2.提交mutation是更改状态的唯一方法，并且这个过程是同步的。

3.异步逻辑都应该封装到action里面。

外部调用mutations: store.commit('SET_AGE', 30);

外部调用actions:store.dispatch('nameAction');

###### vuex中如何获取state的数据呢？

在组件内部中的computed来获取state的数据（computed是实时响应的）。

###### mutations操作来改变state数据

在vuex中，改变状态state的唯一方式是通过提交commit的一个mutations,mutations下的对应函数接收第一个参数state，第二个参数为payload(载荷),payload一般是一个对象，用来记录开发时使用该函数的一些信息。mutations是处理同步的请求。不能处理异步请求的。

###### actions操作mutations异步来改变state数据

actions可以异步操作，actions提交mutations，通过mutations来提交数据的变更。它是异步修改state的状态的。

外部调用方式是 this.$store.dispatch('nameAsyn');

**理解context:**context是和this.$store具有相同的方法和属性的对象。我们可以通过context.state和context.getters来获取state和getters。

**理解dispatch:**它含有异步操作，含义可以理解为“派发”，比如向后台提交数据，可以为this.$store.dispatch('actions方法名', 值);

![image-20200616150049837](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20200616150049837.png)

组件派发任务到actions，actions触发mutations中的方法，然后mutations来改变state中的数据，数据变更后响应推送给组件，组件重新渲染。

vue-router

重定向的意思是，当用户访问/a时，URL将会被替换成/b，然后匹配路由为/b，

/a的别名是/b，意味着，当用户访问/b时，URL会保持为/b，但是路由匹配则为/a，就像用户访问/a一样。

###### Vue中slot的作用？

https://www.cnblogs.com/l-y-c/p/13413359.html

什么是插槽？

我们知道Vue中Child组件的标签的中间是不可以包着什么的。

![img](https://img2020.cnblogs.com/blog/1776897/202007/1776897-20200731155201091-374681511.png)

可是往往在很多时候我们在使用组件的时候总是想在组件间外面自定义一些标签，vue新增了一种插槽机制，叫做作用域插槽。要求的版本是2.1.0+；

插槽，其实就相当于占位符。它在组件中给你的HTML模板占了一个位置，让你来传入一些东西。插槽又分为匿名插槽、具名插槽、作用域插槽。

在2.6.0中，我们为具名插槽和作用域插槽引入了一个新的统一的语法（即v-slot指令）。它取代了slot和slot-scope

###### 匿名插槽

匿名插槽，我们也可以叫它单个插槽或者默认插槽。和具名插槽相对，它是不需要设置name属性的，它隐藏的name属性为default。

father.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814210103243-2117774987.png)

child.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814205652604-841900382.png)

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814210342412-1605968979.png)

匿名插槽，**name**的属性对应的是***default***也可以不写就是默认的意思啦；

在使用的时候还有一个问题要注意的 如果是有2个以上的匿名插槽是会child标签里的内容全部都替换到每个slot；

###### 具名插槽（vue2.6.0+被废弃的slot='name'）

顾名思议就是slot是带有name的，定义：<slot name='header'/> 或者使用简单缩写的定义#header 使用：要用一个template标签包裹

father.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814215614912-2134577339.png)

child.vue![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814215811845-354693148.png)

这里说一下多个具名插槽的使用：多个具名插槽，插槽的位置不是使用插槽的位置而定的，是在定义的时候的位置来替换的

father.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814222032415-869631960.png)

child.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814222212360-1766206425.png)

###### 作用域插槽

就是用来传递数据的插槽

当你想在一个插槽中使用数据时，要注意一个问题作用域的问题，Vue官方文档中说了父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

为了让子组件中的数据在父级的插槽内容中可用，我们可以将数据作为元素的一个特性绑定上去：v-bind:text=‘text’

注意：匿名的作用域插槽和具名的作用域插槽，区别在v-slot:default='接受的名称'（default[匿名的可以不写，具名的相反要写的是对应的name]）

v-slot可以解构接收：解构接收的字段要和传的字段一样才可以 例如：one 对应 v-slot="{one}"

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200815001613374-777519014.png)

效果图：

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200815001754867-1631510469.png)



##### computed、watch、methods的区别：

##### watch属性可以用来监听data属性中数据的变化

同时还可以直接在监听的function中使用参数来获取新值与旧值。其中第一个参数是新值，第二个参数是旧值。同时Watch还可以被用来监听路由router的变化，只是这里的监听的元素是固定的。

Watch 监听 firstName 数据，并根据改变得到的新值，进行某些操作。

![image-20200907145808921](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20200907145808921.png)

注意：上例中，初始 fullName 是没有值的，只有当数据改变时，才会显示。因为 watch 的方法默认是不会执行的，只有当监听数据变化，才会执行。

###### immerdiate 属性

通过声明 immediate 选项为 true，可以立即执行一次 handler。

watch: {

  firstName: {

   handler (newName, oldName) {

​    this.fullName = newName + ' ' + this.lastName

   },

   immediate: true

  }

 },

watch 并不适用于显示某一个数据以及数据的拼装等。watch 用在监听数据变化，做某些指令操作（给后台发数据请求）

###### **deep属性**

不使用 deep 时，当我们改变 obj.a 的值时，watch不能监听到数据变化，默认情况下，handler 只监听属性引用的变化，也就是只监听了一层，但改对象内部的属性是监听不到的。

通过使用 deep: true 进行深入观察，这时，我们监听 obj，会把 obj 下面的属性层层遍历，都加上监听事件，这样做，性能开销也会变大，只要修改 obj 中任意属性值，都会触发 handler。

##### Computed:

computed属性的作用与watch类似，也可以监听属性的变化。只是他会根据他依赖的属性，生成一个属性，让vm对象可以使用这个属性。

如果 computed **所依赖的数据发生改变时，计算属性才会重新计算**，并进行缓存；当改变其他数据时，computed 属性 并不会重新计算，从而提升性能。

computed一般是改变data或者props里面的值为己用。

computed的值不可以在data中定义和赋值

computed是在dom加载后马上执行的。

computed是计算属性，实时响应的，比如你要根据data里一个值随时变化做出一些处理，就用computed。

computed中定义的函数，调用的时候后面不能有小括号，类似属性的调用。

**Methods:**

computed是计算属性，methods是方法。

我们可以将同一函数定义为一个 method 或者一个计算属性。对于最终的结果，两种方式确实是相同的。

不同的是computed计算属性是基于它们的依赖进行缓存的。计算属性computed只有在它的相关依赖发生改变时才会重新求值。这就意味着只要message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。而对于method ，只要发生重新渲染，method 调用总会执行该函数。

当有一个性能开销比较大的的计算属性 A ，它需要遍历一个极大的数组和做大量的计算。然后我们可能有其他的计算属性依赖于 A ，这时候，我们就需要缓存！

总之：数据量大，需要缓存的时候用computed；每次确实需要重新加载，不需要缓存时用methods

###### methods,watch,computed的区别：

1.computed 属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；

2.methods 方法表示一个具体的操作，主要书写业务逻辑；

3.watch 一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是 computed 和 methods 的结合体；

Vue.js在模板表达式中限制了，绑定表达式最多只能有一条表达式，但某些数据需要一条以上的表达式运算实现，此时就可以将此数据放在计算属性（computed）当中。

###### Vuejs中关于computed、methods、watch的区别:

computed：计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。

methods：methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。

watch：是一种更通用的方式来观察和响应 Vue

实例上的数据变动。一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。

通俗来讲：

computed是在HTML DOM加载后马上执行的，如赋值；

而methods则必须要有一定的触发条件才能执行，如点击事件；

watch呢？它用于观察Vue实例上的数据变动。对应一个对象，键是观察表达式，值是对应回调。值也可以是方法名，或者是对象，包含选项。

所以他们的执行顺序为：**默认加载的时候先computed再watch，不执行methods；等触发某一事件后，则是：先methods再watch。**

**computed、watch、methods中定义的方法，在引用时直接使用名字，按属性调用。**

##### 生命周期

![img](https://upload-images.jianshu.io/upload_images/12602393-5310c1449192165f.jpg)

![img](https://upload-images.jianshu.io/upload_images/12602393-1bd8cb609ed7521a.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

- `beforeCreate`:在实例初始化之后，数据观测data observer(`props、data、computed`) 和 `event/watcher` 事件配置之前被调用。
- `created`:实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， `watch/event` 事件回调。然而，挂载阶段还没开始，`$el` 属性目前不可见。
- `beforeMount`:在挂载开始之前被调用：相关的 `render` 函数首次被调用。
- `mounted`: `el` 被新创建的 `vm.$el` 替换，并挂载到实例上去之后调用该钩子。
- `beforeUpdate`:数据更新时调用，发生在虚拟 `DOM` 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
- `updated`：无论是组件本身的数据变更，还是从父组件接收到的 `props` 或者从`vuex`里面拿到的数据有变更，都会触发虚拟 `DOM` 重新渲染和打补丁，并在之后调用 `updated`。
- `beforeDestroy`:实例销毁之前调用。在这一步，实例仍然完全可用。
- `destroyed`:`Vue` 实例销毁后调用。调用后，`Vue` 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。

注意:
 `created`阶段的`ajax`请求与`mounted`请求的区别：前者页面视图未出现，如果请求信息过多，页面会长时间处于白屏状态。

**单个组件的生命周期**

1. 初始化组件时，仅执行了`beforeCreate/Created/beforeMount/mounted`四个钩子函数
2. 当改变data中定义的变量（响应式变量）时，会执行`beforeUpdate/updated`钩子函数
3. 当切换组件（当前组件未缓存）时，会执行`beforeDestory/destroyed`钩子函数
4. 初始化和销毁时的生命钩子函数均只会执行一次，`beforeUpdate/updated`可多次执行

**Vue.nextTick()**

在下次 `DOM` 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 `DOM`。

获取更新后的`DOM`言外之意就是什么操作需要用到了更新后的`DOM`而不能使用之前的`DOM`或者使用更新前的`DOM`会出问题，所以就衍生出了这个获取更新后的    `DOM`的`Vue`方法。所以放在`Vue.nextTick()`回调函数中的执行的应该是会对`DOM`进行操作的 `js`代码。

this.$nextTick()将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。

this.$nextTick()在页面交互，尤其是从后台获取数据后重新生成dom对象之后的操作有很大的优势。

.prevent的作用：阻止默认程序的运行，只执行我们分配给它的方法

###### Vue过滤器 filter

简单介绍一下过滤器，顾名思义，过滤就是一个数据经过了这个过滤之后出来另一样东西，可以是从中取得你想要的，或者给那个数据添加点什么装饰，那么过滤器则是过滤的工具。例如，从['abc','abd','ade']数组中取得包含‘ab’的值，那么可通过过滤器筛选出来‘abc’和‘abd’；把‘Hello’变成‘Hello World’，那么可用过滤器给值‘Hello’后面添加上‘ World’；或者把时间节点改为时间戳等等都可以使用过滤器。

首先，过滤器可在new Vue实例前注册全局的，也可以在组件上写局部。

全局过滤器：

```
Vue.filter('globalFilter',function(value){
	return value+'!!!'
});
```

组件过滤器（局部）：

```
filters:{
	componentFilter:function(value){
		return value + '!!!';
	}
}
```

##### 全局注册时是filter,没有s的。而组件过滤器是filters，是有s的。

###### 使用方法：

``` 
{{'ok' | globalFilter}}
```

###### v-slot的用法：

一句话概括就是v-slot ：后边是插槽名称，=后边是组件内部绑定作用域值的映射。

###### mapGetters

mapGetters(namespace?: string, map:Array<string> | Object<string>: Object)

为组件创建计算属性以返回getter的返回值。第一个参数是可选的，可以是一个命名空间字符串。

###### Vuex Getter

有时候我们需要从store中的state中派生出一些状态，例如对列表进行过滤并计数：

```
computed:{
	doneTodosCount(){
		return this.$store.state.todos.filter(todo=>todo.done).length;
	}
}
```

如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

Vuex允许我们在store中定义"getter"(可以认为是store的计算属性)。就像计算属性一样，getter的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

Getter接受state作为其第一个参数：

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
})
```

通过属性访问：

Getter会暴露为store.getters对象，你可以以属性的形式访问这些值：

```
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```

Getter也可以接受其他getter作为第二个参数：

```
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
```

```
store.getters.doneTodosCount // -> 1
```

我们可以很容易地在任何组件中使用它：

```
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

注意，getter在通过属性访问时是作为Vue的响应式系统的一部分缓存其中的。

通过方法访问：

你也可以通过让getter返回一个函数，来实现给getter传参。在你对store里的数组进行查询时非常有用。

```
getters:{
	getTodoById:(state)=>(id)=>{
		return state.todos.find(todo=>todo.id===id);
	}
}
```

```
store.getters.getTodoById(2) //->{ id: 2, text: '...', done: false }
```

注意，getter在通过方法访问时，每次都会去进行调用，而不会缓存结果。

mapGetters辅助函数

mapGetters辅助甘薯仅仅是将store中的getter映射到局部计算属性：

```
import {mapGetters} from 'vuex';
expoer default{
	computed:{
		//使用对象展开运算符将getter混入computed对象中
		...mapGetters([
			'doneTodosCount',
			'anotherGetter'
		])
	}
}
```

如果你想将一个getter属性另取一个名字，使用对象形式：

```
...mapGetters({
	//把`this.doneCount`映射为`this.$store.getters.doneTodosCount`
	doneConut: 'doneTodosCount'
})
```

###### 在Vue组件中获得Vuex状态

那么我们如何在Vue组件中展示状态呢？由于Vuex的状态存储是响应式的，从store实例中读取状态最简单的方法就是在计算属性中返回某个状态：

```
//创建一个Counter组件
const Counter={
	template:`<div>{{count}}</div>`,
	computed:{
		count(){
			return store.state.count
		}
	}
}
```

每当store.state.count变化的时候，都会重新求取计算属性，并且触发更新相关联的DOM。

mapState辅助函数

mapState(namespace?: string, map:Array<string> | Object<string | function>) ： Object

为组件创建计算属性以返回Vuex store中的状态。

第一个参数是可选的，可以是一个命名空间字符串。

对象形式的第二个参数的成员是一个函数。function(state:any)

```
import {mapState} from 'vuex';
export default{
	computed: {
		mapState({
			conut: state=>state.count,
			//传字符串参数'count'等同于`state=>state.count`
			countAlias:'count',
			//为了能够使用this获取局部状态，必须使用常规函数
			conutPlusLocalState(state){
				return state.count + this.localCount
			}
		})
	}
}
```

当映射的计算属性的名称与state的子节点名称相同时，也可以给mapState传一个字符串数组

```
computed:mapState([
	'count'
	//映射this.count为store.state.count
])
```

###### Action

Action类似于mutation，不同在于：Action提交的是mutation，而不是直接变更状态。Action可以包含任意异步操作。

Action函数接受一个与store实例具有相同方法和属性的context对象，因此你可以调用context.commit提交一个mutation，或者通过context.state和context.getters来获取state和getters。

```
const store= new Vuex.Store({
	state:{
		count: 0
	},
	mutation:{
		increment(state){
			state.count++;
		}
	},
	actions:{
		increment(context){
			context.commit('increment');
		}
	}
})
```

//为什么context对象不是store实例本身？

实践中，我们会经常用到ES2015的参数解构来简化代码（特别是我们需要调用commit很多次的时候）：

```
actions:{
	increment({commit}){
		commit('increment')
	}
}
```

分发Action

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

在组件中分发Action 

在组件中使用this.$store.dispatch('xxx')分发action，或者使用mapActions辅助函数将组件的methods映射为store.dispatch调用（需要先在跟节点注入store）

```
import {mapActions} from 'vuex'
export default{
	methods:{
		...mapActions([
			'increment',
			//将this.increment()映射为this.$store.dispatch('increment')
			'incrementBy',
			//将this.incrementBy(amount)映射为this.$store.dispatch('incrementBy',amount)
		])
		...mapActions({
			add: 'increment'
			//将this.add()映射为this.$store.dispatch('increment')
		})
	}
}
```

mapActions

mapActions(namespace?: string, map: Array<string>|Object<string|function>): Object

创建组件方法分发action

第一个数是可选的，可以是一个命名空间字符串。

对象形式的第二个参数的成员可以是一个函数。function(dispatch:funciton,...args: any[])

###### Vuex

专为Vue.js应用程序开发的状态管理模式，采用集中式存储管理应用的所有组件的状态，以相应的规则保证状态以一种可预测的方式发生变化。

 1.Vuex的状态存储是响应式的。当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会相应地得到高效更新。

2.你不能直接改变store中的状态。改变store中的状态的唯一途径就是显式地提交commit mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

- **store** : 类似容器，包含应用的大部分状态

1. 一个页面只能有一个store
2. 状态存储是响应式的
3. 不能直接改变store中的状态，唯一途径是显示的提交mutations

- **State**:包含所有应用级别的状态的对象```
- **Getters**:在组件内部获取store中状态的函数
- **Mutations**:唯一修改状态的事件回调函数
- **Actions**:包含异步操作、提交mutation改变状态

- Action 类似于 mutation，**不同在于**：

1. Action 提交的是 mutation，而不是直接变更状态。
2. Action 可以包含任意异步操作。

- **Modules**:将store分割成不同的模块
- **我的总结**：数据统一放在state中，然后对于数据的处理，都统一在mutations中，接着在组件内部，绑定函数，触发mutations中的函数。

vue使用vue-shortkey 插件，来使用快捷键

#### Running functions

The code below ensures that the key combination ctrl + alt + o will perform the 'theAction' method.

```
<button v-shortkey="['ctrl', 'alt', 'o']" @shortkey="theAction()">Open</button>
```

The function in the modifier **@shortkey** will be called repeatedly while the key is pressed. To call the function only once, use the **once** modifier

```
<button v-shortkey.once="['ctrl', 'alt', 'o']" @shortkey="theAction()">Open</button>
```

###### vue-事件修饰符（.prevent .stop  .once .capture .self）

(1) .prevent --- 等于javascript的 event.preventDefault()

作用：阻止默认程序的运行

```
<form @submit.prevent='SomeFunction'></form>
```

单独submit点击后会自动进行提交等一系列操作，prevent就可以阻止这些操作，让上面这段代码乖乖执行我们分配给它的SomeFunction。

(2)  .stop 作用：阻止冒泡

什么叫冒泡？冒泡就是子元素的事件传递到父元素。用以下代码为例，点击button后，触发inner-click，但是因为这个button在中间的这个middle里，同时也相当于我们点击了middle这个元素，也就是button的父元素。同理，也相当于点击了outer元素，也就是说执行了inner-click后再执行middle-click紧接着outer-click。**这就叫做冒泡，一个从子元素向父元素传递事件的行为。**

```
<div id="app"> 
　　<div class="outer" @click="outer-click"> 
　　　　<div class="middle" @click="middle-click"> 
　　　　　　<button @click="inner-click">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

```
methods: { 
　　　　inner: function () {
　　　　　　 console.log('inner： 这是最里面的Button' )
　　　　}, 
　　　　middle: function () { 
　　　　　　console.log('middle: 这是中间的Div' )
　　　　}, 
　　　　outer: function () { 
　　　　　　console.log('outer: 这是外面的Div' )
　　　　} 
　　} 
```

运行结果：

```
inner： 这是最里面的Button
middle: 这是中间的Div
outer: 这是外面的Div
```

.stop的存在就相当于阻止事件向父元素传递，保证只执行inner-cli

```
<div id="app"> 
　　<div class="outer" @click="outer-cli"> 
　　　　<div class="middle" @click="middle-cli"> 
　　　　　　<button @click.stop="inner-cli">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

运行结果：

```
inner： 这是最里面的Button
```

同理.stop如果放在middle里面，那么输出结果为：

```
inner： 这是最里面的Button
middle: 这是中间的Div
```

(3) .capture

作用：打乱冒泡顺序

用以下代码为例，**发生click事件时会优先去找你可以传递到的所有父元素中最后一个有.capture的元素**（这里可以传递到middle和outer，最后一个有.capture的元素是outer），然后优先执行这个元素的事件，**紧接着执行倒数第二个有.capture的事件**（middle）,最后再按照正常的冒泡顺序**从自己开始往上执行未经执行的父元素**的click事件。

```
<div id="app"> 
　　<div class="outer" @click.capture="outer"> 
　　　　<div class="middle" @click.capture="middle"> 
　　　　　　<button @click="inner">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

运行结果：

```
outer: 这是外面的Div
middle: 这是中间的Div
inner： 这是最里面的Button
```

其余例子：

```
<div id="app"> 
　　<div class="outer" @click="outer"> 
　　　　<div class="middle" @click.capture="middle"> 
　　　　　　<button @click="inner">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

运行结果：

```
middle: 这是中间的Div
inner： 这是最里面的Button
outer: 这是外面的Div
```

(4) .self 

作用：不让子元素的事件触发自己绑定的事件，**但是不会阻止冒泡**

```
<div id="app"> 
　　<div class="outer" @click="outer"> 
　　　　<div class="middle" @click.self="middle"> 
　　　　　　<button @click="inner">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

这里middle这有一个.self，当我们点击button的时候，先执行inner，传递到middle,但是.self阻止了middle的click事件，继续冒泡到outer，执行outer的click事件。

运行结果：

```
	inner： 这是最里面的Button
	outer: 这是外面的Div
```

(5) .once 事件只会触发一次

```
<button @click.once='inner'>点击我</button>
```

Vue中组件有的时候，属性带:,有的时候不带，什么区别？

冒号后面为变量，会动态变化的值；一般属性后面为常量。

this.$nextTick()将回调延迟到下次DOM更新循环之后执行。在修改数据之后立即使用它，然后等待DOM更新。它跟全局方法vue.nextTick一样，不同的是回调的this自动绑定到调用它的实例上。

this.$nextTick()在页面交互，尤其是从后台获取数据后重新生成dom对象之后的操作有很大的优势。

###### 父组件向子组件传值props

1.定义父组件，父组件传递inputText这个数值给子组件：

```
//父组件
//引入的add-widget组件
//使用 v-bind 的缩写语法通常更简单：
<add-widget :msg-val="msg"> //这里必须要用 - 代替驼峰
// HTML 特性是不区分大小写的。所以，当使用的不是字符串模板，camelCased (驼峰式) 命名的 prop 需要转换为相对应的 kebab-case (短横线隔开式) 命名，当你使用的是字符串模板的时候，则没有这些限制 
</add-widget>
data(){
    return {
        msg: [1,2,3]
    };
}
```

2.定义子组件，子组件通过props方法获取父组件传递过来的值。props中可以定义能接收的数据类型，如果不符合会报错。

```
//子组件通过props来接收数据
//方式1
props: ['msgVal']
//方式2
props: {
	msgVal: Array
}
//方式3
props: {
	msgVal: {
		type: Array, //指定传入的类型
		//type 也可以使一个自定义构造器函数，使用instanceof检测
		default: [0,0,0] //这样可以指定默认的值
	}
}
//注意props会在组件实例创建之前进行校验，所以在default或validator函数里，诸如data、computed或methods等实例属性还无法使用
```

注意：父子组件传值，数据是异步请求，有可能数据渲染时报错

原因：异步请求时，数据还没有获取到但是此时已经渲染节点了

解决方案：可以在父组件需要传递数据的节点加上v-If=isReady(isReady默认为false),异步请求获取数据后（isReady赋值为true）,v-if=isReady

###### 子组件向父组件传递数据

子组件通过$emit方法传递数据

子组件： 子组件绑定个方法，$emit的方法名是父组件的方法名

![img](https://img-blog.csdn.net/20180208172821122)

父组件：触发父组件的方法，然后执行相应的操作

![img](https://img-blog.csdn.net/20180208172849795)

###### 子组件向子组件传递数据

Vue没有直接子对子传参的方法，建议将需要传递数据的子组件，都合并为一个组件。如果一定需要子对子传参，可以先从传到父组件，再传到子组件。或者通过eventBus或vuex(小项目少页面用eventBus,大项目多页面使用vuex)传值。

###### 画面迁移的组件之间传递数据

1）通过路由带参数进行传值，例：两个组件A和B，A组件通过query把orderId传递给B组件

A组件传值写法：

```
this.$router.push({ path: '/componentB', query: {orderId: 123}}) //跳转到B
```

B组件取值的写法

```
this.$route.query.orderId
```

设置路由导航的两种方法：

声明式的导航 <router-link :to='...'>

编程式的导航 router.push(...)

传参的方式又分为查询参数query(+path)和命名路由params(+name)两种方式：

- 命名路由搭配params，刷新页面参数会丢失
- 查询参数搭配query,刷新页面数据不会丢失
- 接受参数使用this.$router后面就是搭配路由的名称就能获取到参数的值

2）通过设置Session Storage缓存的形式进行传递

两个组件A和B，在A组件中设置缓存orderData

```
const orderData = {'orderId': 123, 'price': 88};
sessionStorage.setItem('缓存名称', JSON.stringify(orderData));
```

B组件就可以获取在A中设置的缓存了

```
const dataB= JSON.parse(sessionStorage.getItem('缓存名称'));
```

3）通过provide/inject传值

详情见：https://www.cnblogs.com/vickylinj/p/13368745.html

4）通过$attrs、$listeners传值

​    详情见：https://www.cnblogs.com/vickylinj/p/13376391.html

src/router

![image-20201117140134368](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117140134368.png)

路由匹配时，props:true的作用：

当在routes中设置props:true时，我们在组件中可以通过props:['id']获取路由中的参数（id参数）值，当props:false是无法获取的。

如果我们不使用props属性，那么我们只能通过传统的方式在组件中获取参数数据，那么传统的方式为{{$route.params.id}}，那么传统的方式就是在组件中用到了路由对象，那么组件就和路由耦合了。

变量作为对象的key的使用方法： [] 即变量外加[]

```
var lastWord = 'last word';
    var a = {
      'first word': 'hello',
      [lastWord]: 'world'
    };

    a['first word'] // "hello"
    a[lastWord] // "world"
    a['last word'] // "world"
```

!!value  !!的作用-》类型转换，最终输出的是true或false

数组：使用,声明一个空元素

console.log([,2])

```
<style lang="scss" scoped>
```

lang属性，普通的style标签支持普通的样式，如果想要启用scss或less ,需要为style元素设置lang属性。

scoped 属性，是用来专门用于标签元素内部的，它是通过CSS的属性选择器实现的。scoped只让样式渲染本组件，不会全局渲染。

for (int m = 0; m < lines?.Length; m++)

就问你装不装。?.的意义是，如果前面为空，则返回void.如果不为空则继续下去。是不是同时想起了?: 这种符号。

这叫语法糖，减少你的代码量。

js问号点(可选链)操作符【?.】【??】

相信大家应该都写过类似的代码：

 

let arr = res && res.data && res.data.list

是不是非常不美观，今天介绍的新语法就是为了解决这种问题的

可选链操作符?.

来，用新语法再写一次

let arr = res?.data?.list

是不是很简洁了。

还有，要是想设置默认值怎么办

以前我们是这么写的

let arr = res && res.data || []

现在可以这样

let arr = res?.res?.data ?? []

这个??的意思是当左边的值为null或undefined的时候 就取??右边的值 

###### 全局

当你注册完之后，可以在任何组件中直接使用标签，而不需要在各个组件中引入并局部注册

通常公共组件放在src文件夹下的components文件夹中，这里的组件进行全局注册。

###### 局部

页面中私有的组件放在各自的页面文件夹中并使用下面代码局部注册

```
import ComponentA from './ComponentA'
import ComponentB from './ComponentB'
export default {
  name: "part",
  components: { ComponentA, ComponentB },
}
```

Vue.use()是全局注册插件 ，

Vue.component()是全局注册组件。

![image-20201117142515364](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117142515364.png)

注册组件：

![image-20201117142534691](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117142534691.png)

![image-20201117142552885](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117142552885.png)

![image-20201117142602844](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117142602844.png)

通常我们在vue里面使用别人开发的组件，第一步就是install,第二步在main.js里面引入，第三步Vue.use这个组件。

 

Vue.use()是全局注册插件 ，Vue.component()是全局注册组件。但是为什么会出现进行全局注册组件也使用到了use,而且成功了，自己写的组件会报错呢？

后来看到一篇文章Vue.use自定义自己的全局组件，里面提到了自定义好组件后，对组件又进行了封装，封装成了插件，在方法中通过Vue.component对自定义的组件进行了全局注册。

封装插件

import MyLoading from './Loading.vue'

// 这里是重点 在这个文件中使用install方法来全局注册该组件

const Loading = {

  install: function(Vue){

​    Vue.component('Loading',MyLoading)

  }

}

只要在index.js里规定了install方法，就可以向其他ui组件库那样，使用Vue.use()来全局使用

相信很多人在用Vue使用别人的组件时，会用到 Vue.use() 。例如：Vue.use(VueRouter)、Vue.use(MintUI)。但是用 axios时，就不需要用 Vue.use(axios)，就能直接使用。那这是为什么呐？

因为 axios 没有 install。 https://www.jianshu.com/p/89a05706917a

https://blog.csdn.net/wang729506596/article/details/81018270

当我们封装的插件是这样的：

export const testObj = {

install(Vue, arg) {

​    }

  }

有install方法，那么就要使用Vue.use去初始化这个插件。这样写的好处就是插件需要一开始调用的方法都封装在install里面，更加精简和可拓展性更高。

如果封装的插件是靠这个对象去调用方法，比如axios，那么直接用的就是export default暴露出一个对象，那么就不需要使用Vue.use。

 

两者刚好让我们知道，如果一个插件是必须全部引入，那么使用暴露一整个对象，使用exportdefault或者是暴露一个用install的对象使用Vue.use。而像UI库那么庞大的插件，我们一般按需引入，那么就使用一个一个export的方法，使用花括号{}按需引入。

 

***\*插件\****

插件通常用来为 Vue 添加全局功能。插件的功能范围没有严格的限制——一般有下面几种：

1.添加全局方法或者 property。如：[***\*vue-custom-element\****](https://github.com/karol-f/vue-custom-element)

2.添加全局资源：指令/过滤器/过渡等。如 [***\*vue-touch\****](https://github.com/vuejs/vue-touch)

3.通过全局混入来添加一些组件选项。如 [***\*vue-router\****](https://github.com/vuejs/vue-router)

4.添加 Vue 实例方法，通过把它们添加到 Vue.prototype 上实现。

5.一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 [***\*vue-router\****](https://github.com/vuejs/vue-router)

***\*使用插件\****

通过全局方法 Vue.use() 使用插件。它需要在你调用 new Vue() 启动应用之前完成：

// 调用 `MyPlugin.install(Vue)` 

Vue.use(MyPlugin) 

new Vue({ // ...组件选项 })

Vue.use 会自动阻止多次注册相同插件，届时即使多次调用也只会注册一次该插件。

***\*i18n(其来源是英文单词internationalization的首末字符i和n，18为中间的字符数)是“国际化”的简称。在咨讯领域，国际化(i18n)指让产品（出版物，软件，硬件等）无需做大的改变就能够适应不同的语言和地区的需要。对程序来说，在不修改内部代码的情况下，能根据不同语言及地区显示相应的界面。在全球化的时代，国际化尤为重要，因为产品的潜在用户可能来自世界的各个角落。通常与i18n相关的还有L10n(\*******\*“\*******\*本地化\*******\*”\*******\*的简称)。\****

***\*export default {\****

***\*mixins：[form],\****

***\*}\****

***\*mixins:[form]的作用：\****

vue中提供了一种混合机制--mixins，用来更高效的实现组件内容的复用。

混合 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。
混合对象可以包含任意组件选项。
当组件使用混合对象时，所有混合对象的选项将被混入该组件本身的选项。

组件在引用之后相当于在父组件内开辟了一块单独的空间，来根据父组件props过来的值进行相应的操作，单本质上两者还是泾渭分明，相对独立。

而mixins则是在引入组件之后，则是将组件内部的内容如data等方法、method等属性与父组件相应内容进行合并。相当于在引入后，父组件的各种属性方法都被扩充了。

· 单纯组件引用：
***\*父组件 + 子组件 >>> 父组件 + 子组件\****

· mixins：
***\*父组件 + 子组件 >>> new父组件\****
有点像注册了一个vue的公共方法，可以绑定在多个组件或者多个Vue对象实例中使用。另一点，类似于在原型对象中注册方法，实例对象即组件或者Vue实例对象中，仍然可以定义相同函数名的方法进行覆盖，有点像子类和父类的感觉。

如果在引用mixins的同时，在组件中重复定义相同的方法，则mixins中的方法会被覆盖。

***\*mixins的特点\****

1 方法和参数在各组件中不共享

2 值为对象的选项，如methods,components等，选项会被合并，键冲突的组件会覆盖混入对象的

3 值为函数的选项，如created,mounted等，就会被合并调用，混合对象里的钩子函数在组件里的钩子函数之前调用

与vuex的区别

经过上面的例子之后，他们之间的区别应该很明显了哈~

vuex：用来做状态管理的，里面定义的变量在每个组件中均可以使用和修改，在任一组件中修改此变量的值之后，其他组件中此变量的值也会随之修改。

Mixins：可以定义共用的变量，在每个组件中使用，引入组件中之后，各个变量是相互独立的，值的修改在组件中不会相互影响。

与公共组件的区别

同样明显的区别来再列一遍哈~

组件：在父组件中引入组件，相当于在父组件中给出一片独立的空间供子组件使用，然后根据props来传值，但本质上两者是相对独立的。

Mixins：则是在引入组件之后与组件中的对象和方法进行合并，相当于扩展了父组件的对象与方法，可以理解为形成了一个新的组件。

**顺序很重要. 默认情况下, mixins将首先被调用, 然后是组件, 所以我们可以根据需要来覆盖它(override). 就是说, 组件有最后的发言权. 这只有当冲突发生时才变得非常重要, 在这个时候, 组件必须决定哪一个胜出, 否则一切将被放置在一个数组中执行, mixins相关的放在前面, 然后是组件相关的.**

###### vue v-for循环的用法

1.v-for 循环普通数组

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181205201733535-216659452.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181205201914380-1474410470.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181205202014422-1731723030.png)

2.v-for循环对象数组

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206105740055-1278485321.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206105926902-1376545124.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110006145-1458418636.png)

3.v-for循环对象

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110237124-571883828.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110349160-724955596.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110437789-908913742.png)

4.v-for循环数字

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110638613-739529289.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110712436-7051402.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110755346-747301693.png)

5.v-for中key的使用方式

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206113322631-1352293864.png)

注意：

- v-for循环的时候，key属性只能使用number或string
- key在使用的时候，必须使用v-bind属性绑定的形式，指定key的值
- 在组件中使用v-for循环的时候，或者在一些特殊情况中，如果v-for有问题，必须在使用v-for的同事，指定唯一的字符串/数字类型 :key值。

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206113612245-270150968.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206113743400-603555001.png)
