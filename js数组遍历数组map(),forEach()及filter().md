##### js数组遍历数组map(),forEach()及filter()

共同点：

1. 都是循环遍历数组中的每一项。
2. forEach()和map()里面每一次执行匿名函数都支持3个参数：数组中的当前项item，当前项的索引index，原始数组input。
3. 匿名函数中的this都是指window。
4. 只能遍历数组。

###### forEach() 没有返回值

```
let arr = []; //原数组
arr.forEach(function(value,index,array){
	//do something
})
//demo
let ary = [12, 23, 24, 42, 1];
let res = ary.forEach(function(item,index,input){
	input[index] = item * 10;
})
console.log(res);//--> undefined;  
console.log(ary);//--> 通过数组索引改变了原数组；//[120,230,240,420,10]
```

参数：

1. value数组中的当前项，index当前项的索引，array原始数组
2. 数组中有几项，那么传递进去的匿名回调函数就需要执行几次
3. 理论上这个方法是没有返回值的，仅仅是遍历数组中的每一项，不对原来数组进行修改；但是可以自己通过数组的索引来修改原来的数组

###### map() 有返回值，可以return出来

```
let arr = [];//原数组
arr.map(function(value,index,array){
	//do something
	return xxx;
})
//demo
let ary = [12, 23, 24, 42, 1];
let res = ary.map(function(item,index,input){
	return item*10;
})
console.log(res);//-->[120,230,240,420,10];  原数组拷贝了一份，并进行了修改//-->[12,23,24,42,1]；  原数组并未发生变化
console.log(ary);//-->[12,23,24,42,1]；  原数组并未发生变化
```

参数：

1. value数组中的当前项，index当前项的索引，array原始数组
2. 区别：map的回调函数中支持return返回值；return的是啥，相当于把数组中的这一项变为啥（并不影响原来的数组，只是相当于把原数组克隆一份，把克隆的这一份的数组中的对应项改变了）

###### filter()

```
var filteredArray = array.filter(callback[thisObject]);
//demo
function isBigEnough(element, index, array){
	return (element >=10);
}
var filtered = [12, 5, 8,130, 44].filter(isBigEnough);
console.log(filtered);//[12, 130, 44]
```

参数：

1. callback:要对每个数组元素执行的回调函数
2. thisObject：在执行回调函数时定义的this对象

功能：对数组中的每个元素都执行一次指定的函数（callback），并且创建一个新的数组，该数组元素是所有回调函数执行时返回值为true的原数组元素。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略，同时，新创建的数组不会包含这些元素。

回调函数可以有三个参数：当前元素，当前元素的索引和当前的数组对象。

如参数thisObject被传递进来，它将被当做回调函数（callback）内部的this对象，如果没有传递或者为null，那么将会使用全局对象。

filter不会改变原有数组，记住:只有在回调函数执行前传入的数组元素才有效，在回调函数开始执行后才添加的元素将被忽略，而在回调函数开始执行到最后一个元素这一期间，数组元素被删除或者被更改的，将以回调函数访问到该元素的时间为准，被删除的元素将被忽略。

###### map() + filter()

```
let arr = [10, 20, 3, 40, 5];
let newarr = arr.map(res=>{
	return res + 2;
})
console.log(newarr)//[12, 22, 5, 42, 7]
let filterarr = newarr.filter(it =>it >10).map(it =>it)
console.log(filterarr) // [12, 22, 42]
```

###### some()

```
function isBigEnough(element,index,array){
	return (element >=10);
}
var passed = [2,5,8,1,4].some(isBigEnough);
console.log(passed)//false
passed=[12,5,6,1,4].some(isBigEnough);
console.log(passed)//true
```

对数组中的每个元素都执行一次指定的函数（callback）,直到此函数返回true，如果发现这个元素，some将返回true，如果回调函数对每个元素执行后都返回false，some将返回false。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略。

###### every()

```
function isBigEnough(element, index, array){
	return (element >= 10);
}
var passed = [12, 5, 8, 130, 44].every(isBigEnough);
console.log(passed)//false
passed = [12, 54, 18, 130, 44].every(isBigEnough);
console.log(passed)//true
```

对数组中的每个元素都执行一次指定的函数（callback）,直到此函数返回false，如果发现这个元素，every将返回false，如果回调函数对每个元素执行后都返回true，every将返回true。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略。

###### lastIndexOf()

```
var index = array.lastIndexOf(searchElement[,fromIndex]);
```

当有元素符合条件时，返回当前元素的索引。如果没有发现，就直接返回-1。

searchElement:要搜索的元素

fromIndex:开始搜索的位置，默认为数组的长度（length），在这样的情况下，将搜索所有的数组元素。搜索是反方向进行的。

###### indexOf() 功能与lastIndexOf()一样，搜索是正向进行的

###### match()

```
var str = 'The rain in SPAIN stays mainly in the plain';
var n = str.match(/ain/g);
console.log(n) //["ain", "ain", "ain"]
```

###### replace()

```
var str = 'Visit Microsoft! Visit Microsoft!';
var n = str.replace('Microsoft','Runoob');
console.log(n)//Visit Runoob! Visit Microsoft!

```

###### 数组去重

```
function dedupe(array){
	return [...new Set(array)]
}
var arr = [1,2,2,3,3,4,4,5,5,6,7,6];
console.log(dedupe(arr))// [1, 2, 3, 4, 5, 6, 7]

```

