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

- ###### 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending(进行中)、fulfilled(已成功)和rejected(已失败)。只有异步操作的结果，可以决定当前

  ```
  
  ```

  ###### 是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来。

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

