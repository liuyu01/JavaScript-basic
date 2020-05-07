JS 是单线程的，但是却能执行异步任务，这主要是因为 JS 中存在事件循环（Event Loop）和任务队列（Task Queue）。

![img](http://images2015.cnblogs.com/blog/638135/201607/638135-20160721111145247-341211472.png)

Javascript语言的执行环境是"单线程"（single thread，就是指一次只能完成一件任务。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务，以此类推）。

这种模式的好处是实现起来比较简单，执行环境相对单纯；坏处是只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。常见的浏览器无响应（假死），往往就是因为某一段Javascript代码长时间运行（比如死循环），导致整个页面卡在这个地方，其他任务无法执行。

为了解决这个问题，Javascript语言将任务的执行模式分成两种：同步（Synchronous）和异步（Asynchronous）。

**“同步模式"** 就是上一段的模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的；"异步模式"则完全不同，每一个任务有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则是不等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。

**“异步模式"** 非常重要。在浏览器端，耗时很长的操作都应该异步执行，避免浏览器失去响应，最好的例子就是Ajax操作。在服务器端，"异步模式"甚至是唯一的模式，因为执行环境是单线程的，如果允许同步执行所有http请求，服务器性能会急剧下降，很快就会失去响应。**异步任务不具有”堵塞“效应**。

JS实现异步编程的方法

##### 1）回调函数（Callback）

###### 回调是一个函数被作为一个参数传递到另一个函数里，在那个函数执行完后再执行。

回调函数是异步操作最基本的方法。以下代码就是一个回调函数的例子：

```
ajax(url,()=>{
	//处理逻辑
})
```

但是回调函数有一个致命的弱点，就是容易写出回调地狱（Callback hell）。假设锁哥请求存在依赖性，你可能就会写出如下代码：

```
ajax(url,()=>{
	//处理逻辑
	ajax(url1,()=>{
		//处理逻辑
		ajax(url2,()=>{
			//处理逻辑
		})
	})
})
```

回调函数的优点是简单、容易理解和实现，缺点是不利于代码的阅读和维护，各个部分之间高度耦合，使得程序结构混乱、流程难以追踪（尤其是多个回调函数嵌套的情况），而且每个人任务只能指定一个回调函数。此外它不能使用try catch捕获错误，不能直接return。

##### 2）事件监听

这种方式下，异步任务的执行不取决于代码的顺序，而取决于某个事件是否发生。

下面是两个函数f1和f2，编程的意图是f2必须等到f1执行完成，才能执行。首先，为f1绑定一个事件（这里采用的jQuery的写法）

```
f1.on('done',f2);
```

上面这行代码的意思是，当f1发生done事件，就执行f2。然后，对f1进行改写:

```
function f1(){
	setTimeout(function(){
		//...
		f1.trigger('done');
	},1000);
}
```

上面代码中，f1.trigger('done')表示，执行完成后，立即出发done事件，从而开始执行f2。

这种方法的优点是比较容易理解，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以“去耦合”，有利于实现模块化。缺点是整个程序都要变成事件驱动型，运行流程会变得很不清晰。阅读代码的时候，很难看出主流程。

##### 3）发布订阅

我们假定，存在一个“信号中心”，某个任务执行完成，就向信号中心“发布”（publish）一个信号，其他任务可以向信号中心“订阅”（subscribe）这个信号，从而知道什么时候自己可以开始执行。这就叫做“发布/订阅模式”（publish-subscribe pattern）,又称“观察者模式”（observer pattern）。

首先，f2向信号中心jQuery订阅done信号。

```
jQuery.subscribe('done',f2);
```

然后，f1进行如下改写：

```
function f1(){
	setTimeout(function(){
		//...
		jQuery.publish('done');
	},1000)
}
```

上面代码中，jQuery.publish('done')的意思是，f1执行完成后，向信号中心jQuery发布done信号，从而引发f2的执行。

f2完成执行后，可以取消订阅（unsubscribe）

```
jQuery.unsubscribe('done',f2);
```

这种方法的性质与“事件监听”类似，但是明显优于后者。因为可以通过查看“消息中心”，了解存在多少信号、每个信息有多少订阅者，从而监控程序的运行。

##### 4）Promise/A+

Promise本意是承诺，在程序中的意思就是承诺我过一段时间后会给你一个结果。什么时候会用到过一段时间？答案是异步操作，异步是指可能较长时间才有结果的才做，例如网络请求、读取本地文件等。

Promise是一种更强大的异步处理方式，而且它有统一的API和规范。

```
var promise = loginByPromise('http://www.r9it.com/login.php');
promise.then(function(result){
	//登录成功时处理
}).catch(function(error){
	//登录失败时处理
})
```

有了Promise对象，就可以将异步操作以同步操作的流程表达出来。这样在处理多个异步操作的时候还可以避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，必须通过调用Promise#then和Promise#catch这两个方法来结果，除此之外其他的方法都是不可用的，这样使得异步处理操作更加容易。

在ES6中，可以使用三种办法创建Promise实例（对象）

（1）构造方法

```
let promises = new Promise((resolve,reject)=>{
	resolve();//异步处理
});
```

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。

（2）通过Promise实例的方法，Promise#then方法返回的也是一个Promise对象

```
promise.then(onFulfilled, onRejected);
```

(3)通过Promise的静态方法，Promise.resolve()，Promise.reject()

```
var p = Promise.resolve();
p.then(function(value){
	console.log(value);
});
```

###### Promise的执行流程

- new Promise构造器之后，会返回一个promise对象
- 为promise注册一个事件处理结果的回调函数（resolved）和一个异常处理函数（rejected）

###### Promise的状态

实例化的Promise有三个状态：

Fulfilled: has-resolved，表示成功解决，这时会调用onFulfilled

Rejected:has-rejected,表示解决失败，此时会调用onRejected

Pending:unresolve，表示待解决，既不是resolve也不是reject的状态。也就是promise对象刚被创建后的初始化状态。

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。resolve函数的作用是，将Promise对象的状态从未处理变成处理成功（unresolved=>resolve）,在异步操作成功时调用，并将异步操作的结果作为参数传递出去。reject函数的作用是，将Promise对象的状态从未处理变成处理失败（unresolved=>rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法和catch方法分别指定resolved状态和rejected状态的回调函数。

只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。 

一旦状态改变，就不会再变，任何时候都可以得到这个结果 Promise对象的状态改变，只有两种可能：从 pending 变为 fulfilled 和从 pending 变为 rejected。 只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。 如果改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。 这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的

```
var promise = new Promise((fuck, reject) => {
 resolve("xxxxx");
 //下面这行代码无效，因为前面 resolve 方法已经将 Promise 的状态改为 resolved 了
 reject(new Error()); 
});
 
promise.then((value) => { 
 console.log(value);
})

```

###### Promise 的执行顺序

```
var p = new Promise((resolve, reject) => {
 console.log("start Promise");
 resolve("resolved"); 
});
 
p.then((value) => {
 console.log(value);
}) 
 
console.log("end Promise");

```

当我们在构造 Promise 的时候，构造函数内部的代码是立即执行的

###### promise的链式调用

- 每次调用返回的都是一个新的Promise实例(这就是then可用链式调用的原因)
- 如果then中返回的是一个结果的话会把这个结果传递下一次then中的成功回调
- 如果then中出现异常,会走下一个then的失败回调
- 在 then中使用了return，那么 return 的值会被Promise.resolve() 包装(见例1，2)
- then中可以不传递参数，如果不传递会透到下一个then中(见例3)
- catch 会捕获到没有捕获的异常

它也是存在一些缺点的，比如无法取消 Promise，错误需要通过回调函数捕获。

![img](https://upload-images.jianshu.io/upload_images/3174701-460247a0a17248ca?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

promise是ECMAScript6管理异步代码的标准方式，javascript库使用promise管理ajax，动画和其他典型的异步交互。简单的说，它的思想是：每一个异步任务返回一个promise对象，该对象有一个then方法，允许指定回调函数。比如f1的回调函数f2，可以写成：

```
f1.then(f2);
```

回调函数写成了链式写法，程序的流程可以看得很清楚，而且有一整套的配套方法，可以实现很多强大的功能。

##### 5）生成器Generators/yield

Generator函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不同，Generator最大的特点就是可以控制函数的执行。

- 语法上，首先可以把它理解成，Generator函数是一个状态机，封装了多个内部状态。
- Generator函数除了状态机，还是一个遍历器对象生成函数。
- 可暂停函数，yield可暂停，next方法可启动，每次返回的是yield后的表达式结果。
- yield表达式本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。

##### 6）async/await

使用async/await，你可以轻松地达到之前使用生成器和CO函数所做到的工作，它有如下特点：

- async/await是基于Promise实现的，它不能用于普通函数的回调函数
- async/await 与Promise一样，是非阻塞的
- async/await使得异步代码看起来像同步代码，这正是它的魔力所在

###### 一个函数如果加上async ,那么该函数就会返回一个Promise

```
async function async1(){
	return '1';
}
console.log(async1())//Promise {<resolved>: "1"}
```

await关键字只能用在async定义的函数内。async函数会隐式地返回一个promise,该promise的resolve值就是函数return的值。

JS异步编程进化史：callback->（事件监听->发布订阅）->Promise->Generator->async+await

async/await函数的实现，就是将Generator函数和自动执行器，包装在一个函数里。

async/await可以说是异步终极解决方案了。

（1）async/await函数相对于Promise，优势体现在：

- 处理then的调用链，能够更清晰准确的写出代码
- 并且也能优雅地解决回调地狱问题

当然async/await函数也存在一些缺点，因为await将异步代码改造成了同步代码，如果多个异步代码没有依赖性却使用了await会导致性能上的降低，代码没有依赖性的话，完全可以使用Promise.all的方式。

（2）async/await函数对Generator函数的改进，体现在以下三点：

- 内置执行器 ---Generator函数的执行必须靠执行器，所以才有了co函数库，而async函数自带执行器。也就是说，async函数的执行，与普通函数一模一样，只要一行。

```
var result = asyncReadFile();
```

- 更广泛的使用性。---co函数库约定，yield命令后面只能是Thunk函数或者Promise对象，而async函数的await命令后面，可以跟Promise对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）
- 更好的语义。---async和await,比起星号和yield，语义更清楚了。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。

###### 为什么async/await 更好？

1.简洁  使用async/await 明显节约了不少代码。不需要写.then，不需要写匿名函数处理Promise的resolve值，也不需要定义多余的data变量，还避免了嵌套代码。

2. 错误处理  async/await 让try/catch可以同时处理同步和异步错误。
3. 条件语句
4. 中间值
5. 错误栈  Promise链中返回的错误栈没有给出错误发生位置的线索。更糟糕的是，它会误导我们。然而，async/await中的错误栈会指向错误所在的函数。
6. 调试 最后一点，也是非常重要的一点在于，async/await能够使得代码调试更简单。2个理由使得调试Promise变得非常痛苦：1）不能在返回表达式的箭头函数中设置断点 2）如果你在.then代码块中设置断点，使用Step Over快捷键，调试器不会跳到下一个.then，因为它只会跳过异步代码。

###### 	异步编程的最高境界，就是根本不用关心它是不是异步。

###### async函数的实现

async函数的实现，就是将Generator函数和自动执行器，包装在一个函数里。

```
async function fn(args){
	//...
}
//等同于
function fn(args){
	return spawn(function *(){
		//...
	})
}
```

所有的async函数都可以写成上面的第二种形式，其中的spawn函数就是自动执行器。

注意点：await命令后面的Promise对象，运行结果可能是rejected，所以最好把await命令放在try...catch代码块中。

```
async funciton myFunciton(){
	try{
	  await somethingThatReturnsAPromise();
	}catch(err){
	  console.log(err);
	}
}
//另一种写法
async function myFunction(){
	await somethingThatReturnsAPromise().catch(function(err){
		console.log(err);
	})
}
```

await命令只能用在async函数之中，如果用在普通函数，就会报错。

如果确实希望多个请求并发执行，可以使用Promise.all方法。
