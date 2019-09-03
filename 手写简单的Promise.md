```
function MyPromise(fn){
    const that = this;
    that.state = PENDING;
    that.value = null;
    that.resolvedCallbacks = [];
    that.rejectedCallbacks = [];
    function resolve(value){
        if(that.state == PENDING){
            that.state = RESOLVED;
            that.value = value;
            that.resolvedCallbacks.map(cb=>cb(that.value))
        }
    }

    function reject(value){
        if(that.state == PENDING){
            that.state = REJECTED;
            that.value = value;
            that.rejectedCallbacks.map(cb=>cb(that.value))
        }
    }
    try{
        fn(resolve,reject)
    }catch(e){
        reject(e)
    }
}

MyPromise.prototype.then= function(onFulfilled,onRejected){
    const that = this;
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v=>v;
    onRejected = typeof onRejected === 'function' ? onRejected : r =>{throw r};
    
    if(that.state === PENDING){
        that.resolvedCallbacks.push(onFulfilled);
        that.rejectedCallbacks.push(onRejected);
    }
    if(that.state === RESOLVED){
        onFulfilled(that.value);
    }
    if(that.state===REJECTED){
        onRejected(that.value);
    }
    
};
```
##### Promise对象

它由社区最早提出和实现，ES6将其写进了语言标准，统一了用法，原生提供了Promise对象。

ES6规定，Promise对象是一个构造函数，用来生成Promise实例。

Promise对象有三种状态：

pending：刚刚创建一个Promise实例的时候，表示初始状态

fulfilled：resolve方法调用的时候，表示操纵成功

rejected：reject方法调用的时候，表示操作失败

状态只能从 初始化 -> 成功或者初始化->失败，不能逆向转换，也不能在成功fulfilled和失败rejected之间转换。

then()方法

```
pro.then(function(res){
    //操作成功的处理程序
},function(error){
    //操作失败的处理程序
});
参数是两个函数，第一个用于处理操作成功后的业务，第二个用于处理操作异常后的业务。
```

catch()

```
pro.catch(function(error){
    //操作失败的处理程序
});
```

之所以能够使用链式调用，是因为then和catch方法调用后，都会返回promise对象。

Promise.all()

Promise.all()方法：接受一个数组作为参数，数组的元素是Promise实例对象，当参数中的实例对象的状态都为fulfilled时，Promise.all()才会有返回。

```
Promise.all([pro1,pro2]).then(function(result){
	console.log(result)
})
```

Promise.race()方法

它的参数要求跟Promise.all()方法一样，不同的是，它参数中的Promise实例，只要有一个状态发生变化（不管是成功fulfilled还是异常rejected）,它就会有返回，其他实例中再发生变化，它也不管了。

回调地狱：指的是过多地使用回调函数嵌套，使得调试和维护起来极其的不便。
