This指向

1.在全局环境中的this指向全局对象本身（在浏览器环境下指向window）

2.函数环境中的this，由调用函数的方式决定

​         a.如果函数是独立调用，那this指向undefined。但是在非严格模式中，当this指向undefined时，它会被自动指向全局对象。

​         b.如果函数是被某一个对象调用，那this指向这个对象。

​         注意：箭头函数根本没有自己的this，它的this就是外层代码块的this。它的this始终是固定的。

3.构造函数与原型里的this

​          a.构造函数里的this指向实例

​           b.原型里的this指向实例

const obj = {
    a:10,
    c:this.a + 20,
    fn:function(){
	    return this.a;
    }
};
obj.c; //NaN
obj.fn(); //10



obj = {
	a: 1,
	getA() {
	console.log("getA: ", **this**.a);
	}
};
obj.getA(); //1
x = obj.getA;
x();//undefined
setTimeout(obj.getA, 100);//2   undefined 首先定时器会返回一个执行的ID。其次返回this.a的值。

如果没有特殊指向，setInterval和setTimeout的回调函数中this的指向都是window。这是因为JS的定时器方法是定义在window下的。

##### []语法中的this关键字

```
function fn(){console.log(this)}
function fn2(){alert(1)}
var arr = [fn,fn2]
arr[0]()//这里的this又是什么呢？
```

愉快的转换

```
arr[0]()
假想为 arr.0()
然后转换为 arr.0.call(arr)
那么里面的this就是arr了
```

##### 默认绑定的另一种情况

在函数中以函数作为参数传递，例如setTimeout和setInterval等，这些函数中传递的函数中的this指向，在非严格模式指向的是全局对象。

```
var name = 'koala';
var person = {
	name : '程序员成长指北',
	sayHi : sayHi
};
function sayHi(){
	setTimeout(function(){
		console.log('Hello',this.name);
	})
}
person.sayHi();
setTimeout(function(){
	person.sayHi();
},200);
```

##### 隐式绑定的另一种情况

当有多层对象嵌套调用某个函数的时候，例如对象.对象.函数，this指向的是最后一层对象。

```
function sayHi(){
	console.log('hello,',this.name);
}
var person2 = {
	name : "程序员成长指北",
	sayHi : sayHi
}
var person1 = {
	name : 'koala',
	friend : person2
}
person1.friend.sayHi();//hello, 程序员成长指北
```

##### 显示绑定

显示绑定，通过函数call apply bind 可以修改函数this的指向。call与apply方法都是挂载在Function原型下的方法，所有的函数都能使用。

call 和 apply的第一个参数会绑定到函数体的this上，如果不传参数，非严格模式，this默认绑定到全局对象。

###### call和apply的注意点

这两个方法在调用的时候，如果我们传入数字或者字符串，这两个方法会把传入的参数转成对象类型。

```
var number = 1,string = '程序员成长指北';
function getThisType(){
	var number = 3;
	console.log('this指向内容',this);
	console.log(typeof this);
}
getThisType.call(number);//this指向内容 Number{1} //object
getThisType.apply(string);//this指向内容 String{'程序员成长指北'} //object
```

###### bind函数

bind方法会创建一个新函数。当这个新函数被调用时，bind()的第一个参数将作为它运行时的this，之后的一序列参数将会在传递的实参前传入作为它的参数。

##### this绑定优先级

new绑定 > 显示绑定 > 隐式绑定 > 默认绑定

##### 箭头函数中的this

###### 箭头函数

- 箭头函数中没有arguments
- 箭头函数没有构造函数
- 箭头函数没有原型
- 箭头函数中没有super
- 箭头函数中没有自己的this

箭头函数中的this直接指向的是调用函数的上一层运行时。

##### 自执行函数

什么是自执行函数？自执行函数式在代码定以后，无需调用，会自动执行。

```
(function(){
	console.log('程序员成长指北')
})()
```

或者

```
(function(){
	console.log('程序员成长指北')
}())
```

但是如果使用了箭头函数简化一下就只能使用第一种情况了。使用第二种情况简化会报错。

```
(()=>{
	console.log('程序员成长指北');
})()
```
