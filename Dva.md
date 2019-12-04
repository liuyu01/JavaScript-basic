##### Dva

Reducer:reducer 是一个函数，接受state和action，返回老的或新的state。即：(state,action)=>state

Effects ：put 用于触发action.  call 用于调用异步逻辑，支持promise. select用于从state里获取数据。

##### 数据流向

数据的改变发生通常是通过用户交互行为或者浏览器行为（如路由跳转等）触发的，当此类行为会改变数据的时候可以通过dispatch发起一个action，如果是同步行为会直接通过Reducers改变State，如果是异步行为（副作用）会先触发Effects然后流向Reducers最终改变State，所以在dva中，数据流向非常清晰简明，并且思路基本跟开源社区保持一致。

##### Models

State

State表示Model的状态数据，通常表现为一个javascript对象（当然它可以是任何值）；操作的时候每次都要当作不可变数据（immutable data）来对待，保证每次都是全新对象，没有引用关系，这样才能保证State的独立性，便于测试和追踪变化。

Action

Action是一个普通javascript对象，它是改变State的唯一途径。无论是从UI事件、网络回调，还是WebSocket等数据源所获得的数据，最终都会通过dispatch函数调用一个action，从而改变对应的数据。action必须带有type属性指明具体的行为，其它字段可以自定义，如果要发起一个action需要使用dispatch函数；需要注意的是dispatch是在组件connect Models以后，通过props传入的。

```
dispatch({
	type:'add'
})
```

dispatch函数

dispatching function 是一个用于触发action的函数，action是改变State的唯一途径，但是它只描述了一个行为，而dispatch可以看作是触发这个行为的方式，而Reducer则是描述如何改变数据的。

在dva中，connect Model的组件通过props可以访问到dispatch，可以调用Model中的Reducer或者Effects，常见的形式如下：

```
dispatch({
	type:'user/add',//如果在model外调用，需要添加namespace
	payload: {},//需要传递的信息
})
```

Reducer

Reducer(也称为reducing function)函数接受两个参数:之前已经累积运算的结果和当前要被累积的值，返回的是一个新的累积结果。该函数把一个集合归并成一个单值。

在dva中，reducers聚合积累的结果是当前model的state对象。通过actions中传入的值，与当前reducers中的值进行运算获得新的值（也就是新的state）。需要注意的是Reducer必须是纯函数，所以同样的输入必然得到同样的输出，它们不应该产生任何副作用。并且，每一次的计算都应该使用immutable data，这种特性简单理解就是每次操作都是返回一个全新的数据（独立，纯净），所以热重载和时间旅行这些功能才能够使用。

Effect

Effect被称为副作用，在我们的应用中，最常见的就是异步操作。它来自于函数编程的概念，之所以叫副作用是因为它使得我们的函数变得不纯，同样的输入不一定获得同样的输出。

dva为了控制副作用的操作，底层引入了redux-saga做异步流程控制，由于采用了generator的相关概念，所以将异步转成同步写法，从而将effects转为纯函数。

Subscription

Subscriptions是一种从源获取数据的方法，它来自于elm。

Subscription语义是订阅，用于订阅一个数据源，然后根据条件dispatch需要的action。数据源可以是当前的时间、服务器的websocket连接、keyboard输入、geolocation变化、history路由变化等等。

##### Router

这里的路由通常指的是前端路由，由于我们的应用现在通常是单页应用，所以需要前端代码来控制路由逻辑，通过浏览器提供的History API 可以监听浏览器url的变化，从而控制路由相关操作。

dva实例提供了router方法来控制路由，使用的是react-router。

##### 目前最流行的数据流方案

路由：React-Router

架构：Redux

异步操作：Redux-saga

##### dva是什么

dva是体验技术部开发的React应用框架，将上面三个React工具库包装在一起，简化了API，让开发React应用更加方便和快捷。

dva = React-router + Redux + Redux-saga

##### 数据流图 

![img](https://zos.alipayobjects.com/rmsportal/hUFIivoOFjVmwNXjjfPE.png)

State:一个对象，保存整个应用状态

View:React组件构成的视图层

Action:一个对象，描述事件

connect方法，一个函数，绑定State到View

dispatch方法：一个函数，发送Action到State

##### State和View

State是储存数据的地方，收到Action以后，会更新数据。

View就是React组件构成的UI层，从State取数据后，渲染成HTML代码。只要State有变化，View就会自动更新。

##### Action

Action是用来描述UI事件的一个对象。

```
{
 type: 'click-submit-button',
 payload: this.form.data
}
```

##### connect方法

connect是一个函数，绑定State到View.

```
 import {connect} from 'dva';
 funciton mapStateToProps(state){
 	return {todos: state.todos}
 }
 connect(mapStateToProps)(App);
```

connect方法返回的也是一个React组件，通常称为容器组件。因为它是原始UI组件的容器，即在外面包了一层State。

connect方法传入的第一个参数是mapStateToProps函数，mapStateToProps函数会返回一个对象，用于建立State到Props的映射关系。

##### dispatch方法

dispatch是一个函数方法，用来将Action发送给State。

```
dispatch({
 type: 'click-submit-button',
 payload: this.form.data
})
```

dispatch方法从哪里来？被connect的Component会自动在props中拥有dispatch方法。

##### Model对象的属性

namespace:当前Model的名称。整个应用的State，由多个小的Model的State以namespace为key合成

state:该Model当前的状态。

reducers:Action处理器，处理同步动作，用来算出最新的State。

effects:action处理器，处理异步动作

Reducer

Reducer是Action处理器，用来处理同步操作，可以看作是state的计算器。它的作用是根据Action，从上一个State算出当前State。

##### Effect

Action处理器，处理异步操作，基于Redux-saga实现。Effect指的是副作用。根据函数式编程，计算以外的操作都属于Effect，典型的就是I/O操作、数据库读写。

##### Generator函数

Effect是一个Generator函数，内部使用yield关键字，标识每一步的操作（不管是异步或同步）。

##### call和put

dva提供多个effect函数内部的处理函数，比较常用的是call 和put。

call：执行异步函数

put：发出一个Action，类似于dispatch
