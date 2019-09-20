function MyPromise(fn){

​    const that = this;

​    that.state = PENDING;

​    that.value = null;

​    that.resolvedCallbacks = [];

​    that.rejectedCallbacks = [];

​    function resolve(value){

​        if(that.state == PENDING){

​            that.state = RESOLVED;

​            that.value = value;

​            that.resolvedCallbacks.map(cb=>cb(that.value))

​        }

​    }



​    function reject(value){

​        if(that.state == PENDING){

​            that.state = REJECTED;

​            that.value = value;

​            that.rejectedCallbacks.map(cb=>cb(that.value))

​        }

​    }

​    try{

​        fn(resolve,reject)

​    }catch(e){

​        reject(e)

​    }

}

MyPromise.prototype.then= function(onFulfilled,onRejected){

​    const that = this;

​    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v=>v;

​    onRejected = typeof onRejected === 'function' ? onRejected : r =>{throw r};

​    

​    if(that.state === PENDING){

​        that.resolvedCallbacks.push(onFulfilled);

​        that.rejectedCallbacks.push(onRejected);

​    }

​    if(that.state === RESOLVED){

​        onFulfilled(that.value);

​    }

​    if(that.state===REJECTED){

​        onRejected(that.value);

​    }

​    

};



##### Promise对象

它由社区最早提出和实现，ES6将其写进了语言标准，统一了用法，原生提供了Promise对象。

ES6规定，Promise对象是一个构造函数，用来生成Promise实例。

Promise对象有三种状态：

pending：刚刚创建一个Promise实例的时候，表示初始状态

fulfilled：resolve方法调用的时候，表示操纵成功

rejected：reject方法调用的时候，表示操作失败

状态只能从 初始化 -> 成功或者初始化->失败，不能逆向转换，也不能在成功fulfilled和失败rejected之间转换。

###### then()方法

```
pro.then(function(res){
    //操作成功的处理程序
},function(error){
    //操作失败的处理程序
});
参数是两个函数，第一个用于处理操作成功后的业务，第二个用于处理操作异常后的业务。第二参数是可选的，不一定要提供。这两个函数都接受Promise对象传出的值作为参数。
```

then()方法指定的回调函数，如果运行中抛出错误，也会被catch()方法捕获。

###### catch()

 对于操作异常的程序。只接受一个参数，用于处理操作异常后的业务。

```
pro.catch(function(error){
    //操作失败的处理程序
});
```

之所以能够使用链式调用，是因为then和catch方法调用后，都会返回promise对象。

Promise在resolve语句后面，再抛出错误，不会被捕获，等于没有抛出。因为Promise的状态一旦改变，就永久保持该状态，不会再变了。

###### Promise.all()

Promise.all()方法：接受一个数组作为参数，数组的元素是Promise实例对象，当参数中的实例对象的状态都为fulfilled时，Promise.all()才会有返回。如果参数中promise有一个失败，此实例回调失败，失败原因的是第一个失败promise的结果。

```
Promise.all([pro1,pro2]).then(function(result){
	console.log(result)
})
```

注意：如果传入的promise中有一个失败（rejected），Promise.all异步地将失败的那个结果给失败状态的回调函数，而不管其他promise是否完成。

###### Promise.race()方法

它的参数要求跟Promise.all()方法一样，不同的是，它参数中的Promise实例，只要有一个状态发生变化（不管是成功fulfilled还是异常rejected）,它就会有返回，其他实例中再发生变化，它也不管了。那个率先改变的Promise实例的返回值，就传递给Promise的回调函数。

回调地狱：指的是过多地使用回调函数嵌套，使得调试和维护起来极其的不便。

###### Promise.finally()方法

finally()用于指定不管Promise对象最后状态如何，都会执行的操作。该方法是ES2018引入标准的。这避免了同样的语句需要在then()和catch()中各写一次的情况。

```
promise.then(result=>{})
.catch(error=>{})
.finally(()=>{})
```

上面代码中，不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数。

###### Promise.resolve()将现有对象转为Promise对象

```
Promise.resolve('foo')
//等价于
new Promise(resolve=>resolve('foo'))
```

Promise.resolve(value)方法返回一个以给定值解析后的Promise对象。如果该值为promise,返回这个promise；如果这个值是thenable(即带有‘then’方法)，返回的promise会‘跟随’这个thenable的对象，采用它的最终状态；否则返回的promise将以此值完成。此函数将类promise对象的多层嵌套展平。

###### Promise.reject(reason)方法返回一个带有拒绝原因reason参数的Promise对象。

Promise是异步编程的一种解决方案，比传统的回调函数和事件更合理和强大。

###### Promise对象有以下两个特点：

- 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending(进行中)、fulfilled(已成功)和rejected(已失败)。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来。
- 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能，从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为resolved(已定型)。如果改变已经发生了。你再对Promise对象添加回调函数，也会立即得到这个结果。

有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调地狱。

###### 缺点：

1.首先，无法取消Promise，一旦新建就会立即执行，无法中途取消。

2.其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。

3.当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）

Promise实例生成以后，可以用then()方法分别指定resolved状态和rejected状态的回调函数，用于绑定处理操作后的处理程序。

###### Promise新建后就会立即执行

```
let promise = new Promise(funtion(resolve,reject){
	console.log('Promise')
	resolve();
});
promise.then(function(){
	console.log('resolved');
})
console.log('Hi');
//Promise
//Hi
//resolved
```

Promise新建后立即执行，所以首先输出的是Promise。然后，then()方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以resolved最后输出。
由此Promise对象还是很好用的，对于异步的流程的控制得到了大大改善，通过.them()的方法可进行链式调用。可是.then() .catch()的使用也导致代码非常难看，嵌套也很深，所以async/await就出来了。

async/await是JavaScript编写异步程序的新方法。以往的异步方法无外乎回调函数和Promise。但是async/await建立于Promise之上。

##### 什么是async?

ES2017标准引入了async函数，使得异步操作变得更加方便。

async 函数是Generator函数的语法糖。使用关键字async来表示，在函数内部使用await来表示异步。

async函数就是将Generator函数的星号（*）替换成async，将yield替换成await，仅此而已。

相较于Generator,async函数的改进在于下面四点：

- 内置执行器。Generator函数的执行必须依靠执行器，而async函数自带执行器，调用方式跟普通函数的调用一样
- 更好的语义。async和await相较于* 和 yield更加语义化。async表示函数里异步操作，await表示紧跟在后面的表达式需要等待结果。
- 更广的使用性。co模块约定，yield命令后面只能是Thunk函数或Promise对象。而async函数的await命令后面则可以是Promise或者原始类型的值（Number,string，boolean，但这时等同于同步操作）
- 返回值是Promise。async函数返回值是Promise对象，比Generator函数返回的Iterator对象方便，可以直接使用then()方法进行调用。

###### 基本用法：

async函数返回一个Promise对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

async是ES7新出的特性，表明当前函数是异步函数，不会阻塞线程导致后续代码停止运行。

```
async function asyncFn(){
	return '我后执行';
}
asyncFn().then(result => {
	console.log(result);
})
console.log('我先执行');
我先执行
我后执行
```

虽然是上面asyncFn()先执行，但是已经被定义异步函数了，不会影响后续函数的执行。

###### 特性：

async定义的函数内部会默认返回一个promise对象，如果函数内部抛出异常或者是返回reject，都会使函数的promise状态为失败reject。

async函数接收到返回的值，发现不是异常或者reject，则判定成功，这里可以return各种数据类型的值，false ，NaN ,undefined ...总之，都是resolve

但是返回如下结果会使async函数判定失败reject:

- 内部含有直接使用并且未声明的变量或者函数
- 内部抛出一个错误 throw new Error 或者返回reject状态 return Promise.reject('执行失败')
- 函数方法执行出错等等。。。

还有一点，在async里，必须将结果return回来，不然的话不管是执行reject还是resolved的值都为undefined，建议使用箭头函数。

###### 语法：

async函数的语法规则总体上比较简单，难点是错误处理机制。

async函数返回一个Promise对象。

async函数内部return语句返回的值，会成为then方法回调函数的参数。

```
async function f(){
	return 'hello world';
}
f().then(v=>console.log(v))
//hello world
```

上面代码中，函数f内部return命令返回的值，会被then方法回调函数接收到。

async函数内部抛出错误，会导致返回的Promise对象变为reject状态。抛出的错误对象会被catch方法回调函数接收到。

async函数返回的Promise对象，必须等到内部所有await命令后面的Promise对象执行完，才会发生状态改变，除非遇到return语句或抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。

##### await 是什么？

await意思是async wait(异步等待)。这个关键字只能在使用async定义的函数里面使用。任何async函数都会默认返回promise，并且这个promise解析的值都将会是这个函数的返回值，而async函数必须等到内部所有的await命令的Promise对象执行完，才会发生状态改变。

###### 打个比方，await是学生，async是校车，必须等人齐了再开车。

就是说，必须等所有await函数执行完毕后，才会告诉promise我成功了还是失败了，执行then或者catch

很多人以为await会一直等待之后的表达式执行完之后才会继续执行后面的代码，实际上await是一个让出线程的标志。await后面的函数会先执行一遍（比如await Fn()的Fn,并非是下一行代码），然后就会跳出整个async函数来执行后面js栈的代码。等本轮事件循环执行完了之后又会跳回到async函数中等待await后面表达式的返回值，如果返回值为非promise则继续执行async函数后面的代码，否则将返回的promise放入Promise队列。

正常情况下，await命令后面跟着的是Promise，如果不是的话，也会被转换成一个立即resolve的Promise。

await命令后面的Promise对象如果变为reject状态，则reject的参数会被catch方法的回调函数接收到。

async里如果有多个await函数的时候，如果其中任一一个抛出异常或者报错了，都会导致函数停止执行，直接reject.

有时，我们希望即使前一个异步操作失败，也不要中断后面的异步操作。这时可以将第一个await放在try...catch结构里面，这样不管这个异步操作是否成功，第二个await都会执行。另一种方法是await后面的Promise对象再跟一个catch方法，处理前面可能出现的错误。

###### 错误处理

如果await后面的异步操作出错，那么等同于async函数返回的Promise对象被reject。

```
async function f(){
	await new Promise(function(resolve, reject){
		throw new Error(' 出错了');
	});
}
f().then(v=>console.log(v))
   .catch(e=>console.log(e)) 
//Error: 出错了
```

上面代码中，async函数f执行后，await后面的Promise对象会抛出一个错误对象，导致catch方法的回调函数被调用，它的参数就是抛出的错误对象。

###### 注意点

第一点，前面已经说过，await命令后面的Promise对象，运行结果可能是rejected，所以最好把await命令放在try...catch代码块中。

第二点，多个await命令后面的异步操作，如果不存在依赖关系，最好让它们同时触发。

第三点，await命令只能用在async函数之中，如果用在普通函数，就会报错。

###### async函数的实现原理

async函数的实现原理，就是将Generator函数和自动执行器，包装在一个函数里。

