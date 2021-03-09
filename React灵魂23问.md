##### React灵魂23问

###### 1.setState是异步还是同步？

1.合成事件中是异步（合成事件：react为了解决跨平台，兼容性问题，自己封装了一套事件机制，代理了原生的事件，像在jsx中常见的onClick、onChange这些都是合成事件）

2.钩子函数中的是异步

3.原生事件中是同步（原生事件：是指非react合成事件，原生自带的事件监听addEventListener，或者也可以用原生js、jq直接document.querySelector().onclick这种绑定事件的形式都属于原生事件。）

4.setTimeout中是同步（在setTimeout中去setState并不算是一个单独的场景，它是随着你外层去决定的，因为你可以在合成事件中setTimeout，可以在钩子函数中setTimeout，也可以在原生事件setTimeout，但是不管是哪个场景下，基于event loop的模型下，setTimeout中里去setState总能拿到最新的state值）。

###### setState中的批量更新

在setState的时候react内部会创建一个updateQueue，通过firstUpdate、lastUpdate、lastUpdate.next去维护一个更新的队列，在最终的performWork中，相同的key会被覆盖，只会对最后一次的setState进行更新。

1、setState只在合成事件和钩子函数中是异步的，在原生事件和setTimeout中都是同步的。

2、setState的异步并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的“异步”，当然可以通过第二个参数setState(partialState, callback)中的callback拿到更新后的结果。

3、setState的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout中不会批量更新，在“异步”中如果对同一个值进行多次setState，setState的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时setState多个不同的值，在更新时会对其进行合并批量更新。

###### 2.react@16.4+的生命周期

![preview](https://pic3.zhimg.com/v2-34344a720b3e38ff64e2a26c2089680a_r.jpg)

###### 3.useEffect(fn, [])和componentDidMount有什么差异？

useEffect会捕获props和state。所以即便在回调函数里，你拿到的还是初始的props和state。如果想得到“最新”的值，可以使用ref。

###### 4.hooks为什么不能放在条件判断里？

以useState为例，在react内部，每个组件（Fiber）的hooks都是以链表的形式存在memoizeState属性中。update阶段，每次调用useState，链表就会执行next向后移动一步。如果将useState写在条件判断中，假设条件判断不成立，没有执行里面的useState方法，会导致接下来所有的useState的取值出现偏移，从而导致异常发生。

###### 5.fiber是什么？

React Fiber是一种基于浏览器的单线程调度算法。

React Fiber 用类似requestIdleCallback的机制来做异步diff。但是之前数据结构不支持这样的实现异步diff，于是React实现了一个类似链表的数据结构，将原来的递归diff变成了现在的遍历diff，这样就能做到异步可更新了。

![preview](https://pic4.zhimg.com/v2-8e691022bf76747599b85f7c7797206b_r.jpg)

###### 6.聊一聊diff算法

传统diff算法的时间复杂度是O(n^3)，这在前端render中是不可接受的。为了降低时间复杂度，react的diff算法做了一些妥协，放弃了最优解，最终将时间复杂度降低到了O(n)。

那么react diff算法做了哪些妥协呢？参考如下：

1.tree diff:只对比同一层的dom节点，忽略dom节点的跨层级移动

如下图，react只会对相同颜色方框内的DOM节点进行比较，即同一个父节点下的所有子节点。当发现节点不存在时，则该节点及其子节点会被完全删除掉，不会用于进一步的比较。

这样只需要对树进行一次遍历，便能完成整个DOM树的比较。

![preview](https://pic3.zhimg.com/v2-d2cb592bc4da53a85dbf6cef2dd7c9ee_r.jpg)

这就意味着，如果dom节点发生了跨层级移动，react会删除旧的节点，生成新的节点，而不会复用。

2.component diff:如果不是同一类型的组件，会删除旧的组件，创建新的组件

![preview](https://pic2.zhimg.com/v2-d8d3646da454398e0defacb61b5e1081_r.jpg)

3.element diff:对于同一层级的一组子节点，需要通过唯一id进行区分。

如果没有id来进行区分，一旦有插入动作，会导致插入位置之后的列表全部重新渲染。这也是为什么渲染列表时为什么要使用唯一的key。

###### 7.调用setState之后发生了什么？

1.在setState的时候，React会为当前节点创建一个updateQueue的更新列队。

2.然后会触发reconciliation过程，在这个过程中，会使用名为Fiber的调度算法，开始生成新的Fiber树，Fiber算法的最大特点是可以做到异步中断的执行。

3.然后React Scheduler会根据优先级高低，先执行优先级高的节点，具体是执行doWork方法。

4.在doWork方法中，React会执行一遍updateQueue中的方法，以获得新的节点。然后对比新旧节点，为老节点打上更新、插入、替换等Tag。

5.当前节点doWork完成后，会执行performUnitOfWork方法获得最新节点，然后再重复上面的过程。

6.当所有节点都doWork完成后，会触发commitRoot方法，React进入commit阶段。

7.在commit阶段中，React会根据前面为各个节点打的Tag，一次性更新整个dom元素。

###### 8.为什么虚拟dom会提高性能？

虚拟dom相当于在JS和真实dom中间加了一个缓存，利用diff算法避免了没有必要的dom操作，从而提高性能。

###### 9.错误边界是什么？它有什么用？

在React中，如果任何一个组件发生错误，它将破坏整个组件树，导致整页白屏。这时候我们可以用错误边界优雅地降级处理这些错误。

###### 10.什么事Portals?

Portal提供了一种将子节点渲染到存在于父组件以外的DOM节点的优秀的方案。

```
ReactDOM.createPortal(child, container)
```

###### 11.React组件间有哪些通信方式？

1.父组件向子组件通信

通过props传递

2.子组件向父组件通信

主动调用通过props传过来的方法，并将想要传递的信息，作为参数，传递到父组件的作用域中

3.跨层级通信

1）使用react自带的Context进行通信，createContext创建上下文，useContext使用上下文。

2）使用Redux或者Mobx等状态管理库

3）使用订阅发布模式

###### 12.React父组件如何调用子组件中的方法？

1、如果是在方法组件中调用子组件（`>= react@16.8`），可以使用 `useRef` 和 `useImperativeHandle`:

```text
const { forwardRef, useRef, useImperativeHandle } = React;

const Child = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    getAlert() {
      alert("getAlert from Child");
    }
  }));
  return <h1>Hi</h1>;
});

const Parent = () => {
  const childRef = useRef();
  return (
    <div>
      <Child ref={childRef} />
      <button onClick={() => childRef.current.getAlert()}>Click</button>
    </div>
  );
};
```

2、如果是在类组件中调用子组件（`>= react@16.4`），可以使用 `createRef`:

```text
const { Component } = React;

class Parent extends Component {
  constructor(props) {
    super(props);
    this.child = React.createRef();
  }

  onClick = () => {
    this.child.current.getAlert();
  };

  render() {
    return (
      <div>
        <Child ref={this.child} />
        <button onClick={this.onClick}>Click</button>
      </div>
    );
  }
}

class Child extends Component {
  getAlert() {
    alert('getAlert from Child');
  }

  render() {
    return <h1>Hello</h1>;
  }
}
```

###### 13.React有哪些优化性能的手段？

###### 类组件中的优化手段

1）使用纯组件PureComponent作为基类

2）使用React.memo高阶函数包装组件

3）使用shouldComponentUpdate生命函数来定义渲染逻辑

###### 方法组件中的优化手段

1）使用useMemo

2)使用useCallBack

###### 其他方式

1.在列表需要频繁变动时，使用唯一id作为key，而不是数组下标。

2.必要时通过改变CSS样式隐藏显示组件，而不是通过条件判断显示隐藏组件

3.使用Suspense和lazy进行懒加载

###### 14.为什么React元素有一个$$typeof属性？

![preview](https://pic2.zhimg.com/v2-1d9e3361bd9ec0f42db7af5192497eb1_r.jpg)

目的是为了防止XSS攻击。因为Symbol无法被序列化，所以React可以通过有没有$$typeof属性来断出当前的element对象是从数据库来的还是自己生成的。

如果没有$$typeof这个属性，react会拒绝处理该元素。

###### 15.React如何区分Class组件和Function组件？

一般的方式是借助typeof和Function.prototype.toString来判断当前是不是class,如下：

```
function isClass(func) {
  return typeof func === 'function'
    && /^class\s/.test(Function.prototype.toString.call(func));
}
```

但是这个方式有它的局限性，因为如果用了babel等转换工具，将class写法全部转为function写法，上面的判断就会失效。

React区分Class组件和Function组件的方式很巧妙，由于所有的类组件都要继承React.Component,所以只要判断原型链上是否有React.Component就可以了。

```
AComponent.prototype instanceof React.Component
```

###### 16.HTML和React事件处理有什么区别？

在HTML中事件名必须小写：

```
<button onclick='activateLasers'>
```

而在React中需要遵循驼峰写法：

```
<button onClick={activateLasers}>
```

在HTML中可以返回false以阻止默认的行为：

```
<a href="#" onclick='console.log("The link was clicked."); return false;'></a>
```

而在React中必须明确地调用preventDefault();

```
function handleClick(event){
	event.preventDefault();
	console.log('The link was clicked.');
}
```

###### 17.什么事suspense组件？

Suspense让组件“等待”某个异步操作，直到该异步操作结束即可渲染。

```
const resource = fetchProfileData();

function ProfilePage() {
  return (
    <Suspense fallback={<h1>Loading profile...</h1>}>
      <ProfileDetails />
      <Suspense fallback={<h1>Loading posts...</h1>}>
        <ProfileTimeline />
      </Suspense>
    </Suspense>
  );
}

function ProfileDetails() {
  // 尝试读取用户信息，尽管该数据可能尚未加载
  const user = resource.user.read();
  return <h1>{user.name}</h1>;
}

function ProfileTimeline() {
  // 尝试读取博文信息，尽管该部分数据可能尚未加载
  const posts = resource.posts.read();
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.text}</li>
      ))}
    </ul>
  );
}
Suspense 也可以用于懒加载，参考下面的代码：

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

###### 18.为什么JSX中的组件名要以大写字母开头？

因为React要直到当前渲染的是组件还是HTML元素。

###### 19.redux是什么？

Redux是一个为JavaScript应用设计的，可预测的状态容器。

它解决了如下问题：

- 跨层级组件之间的数据传递变得很容易
- 所有对状态的改变都需要dispatch，使得整个数据的改变可追踪，方便排查问题。

但是它也有缺点：

- 概念偏移，理解起来不容易
- 样板代码太多

###### 20.react-redux的实现原理？

通过redux和 react context配合使用，并借助高阶函数，实现了 react-redux。

###### 21.redux和mobx的区别？

得益于Mobx的observable，使用mobx可以做到精准更新；对应的Redux是用dispatch进行广播，通过Provider和connect来比对前后差别控制更新粒度

###### 22.redux有哪些异步中间件？

1.redux-thunk

源代码简短优雅，上手简单

2.redux-saga

借助JS的generator来处理异步，避免了回调的问题

3.redux-observable

借助了RxJS流的思想以及其各种强大的操作符，来处理异步问题
