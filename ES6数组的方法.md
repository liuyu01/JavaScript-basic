##### 1.reduce  递归累加  迭代数组的所有项，累加器，数组中的每个值（从左到右）合并，最终计算为一个值

const nums = [1, 2, 3];

let value = 0;

for (let i = 0; i < nums.length; i++) {

​    value += nums[i];

}

ES6实现：

const nums = [1, 2, 3];

const value = nums.reduce((ac, next) => ac + next, 0);

console.log(value); //6

=》

const nums = [1,2,3];

const initialValue = 0;

//可以创建一个函数，它接受一个累加器(`acc`)和一个项(`item`)。累加器是在上一次调用中返回的累积值(或者是 `initialValue`)，是下一个回调的输入值。

const reducer = function(acc,item){

  return acc + item;

};

const total = nums.reduce(reducer,initialValue);

console.log(total)

`reduce` 可以理解为「归一」，意为海纳百川，万剑归一，完整的结构是 `Array.prototype.reduce(callbackfn[, initialValue])`，这里第二个参数并不是 thisArg 了，而是初始值 `initialValue`

如果没有提供 `initialValue`，那么第一次调用 `callback` 函数时，`accumulator` 使用原数组中的第一个元素，`currentValue` 即是数组中的第二个元素。

如果提供了 `initialValue`，`accumulator` 将使用这个初始值，`currentValue` 使用原数组中的第一个元素。

在没有初始值的空数组上调用 `reduce` 将报错。

##### 2.forEach 批量操作

var potatoes = [{ id: '1001', weight: 50 },

{ id: '1002', weight: 80 },

{ id: '1003', weight: 120 },

{ id: '1004', weight: 40 },

{ id: '1005', weight: 110 },

{ id: '1006', weight: 60 }]



potatoes.forEach(potato =>  potato.weight += 20 )

console.log(potatos)

##### 3.map 映射

map最适合做的事是映射，生成原始数据的特征信息的map。返回一个新数组

相比较之下，forEach是没有返回值的，即便加上return也没有用

w = potatoes.forEach(potato =>  {return potato.weight += 20 })

console.log(w) //undefined

而map有返回值，可以把土豆的重量统计汇总，整理一份表格

w = potatoes.map(potato => {return potato.weight +=20 })

console.log(w) //[ 70, 100, 140, 60, 130, 80 ]

`map` 函数接收两个参数，一个是必填项回调函数，另一个是可选项 callbackfn 函数执行时的 this 值。

`map` 方法的主要功能就是把原数组中的每个元素按顺序执行一次 `callbackfn` 函数，并且把所有返回的结果组合在一起生成一个新的数组，`map` 方法的返回值就是这个新数组。

##### 4.filter 过滤筛选

var bigOnes = potatoes.filter(potato =>{return potato.weight > 100 })

console.log(bigOnes)//[ { id: '1003', weight: 120 }, { id: '1005', weight: 110 } ]

返回一个新的对象数组，并不会改变原数组

filter(callbackfn[, thisArg])

`callbackfn` 执行结果如果是 true 就返回当前元素，false 则不返回，返回的所有元素组合在一起生成新数组，并返回。如果没有任何元素通过测试，则返回空数组。

##### 5.some 有符合

当只需要判断数组中有没有符合条件的时候（一个就行） 就需要我们的`some`方法登场了

var hasBig = potatoes.some(potato=>{return potato.weight > 100})

console.log(hasBig)  // true

并不会全部遍历，不做多余的活（性能优良）

##### 6.every 全符合

`every`对每一个元素执行一个`callback`，直到它找到一个使 `callback` 返回 `false`的元素，就返回`false`，直到遍历完成也没有返回`false`的话，就返回`true`

var allBig = potatoes.every(potato =>{return potato.weight > 100 })

console.log(allBig) // false

##### 7.find 返回一个符合的

`find`和`some`很类似，都是寻找符合条件的，有一个就可以 不过`some`进去搜罗了一圈回来报了个“有”（`true`），而`find`则把那个土豆抱了出来（返回第一个符合条件的对象）

var big = potatoes.find(potato =>{return potato.weight > 100})

console.log(big) //{id: "1003", weight: 120}
语法：array.find(function(currentValue, index, arr), thisValue);

##### 8.findIndex 返回序号

`findIndex`返回第一个符合条件的索引号

var i = potatoes.findIndex(potato => {return potato.weight > 100})

console.log(i) //2

##### 9.Array.from()

将一个类数组对象或者可遍历对象转换成一个真正的数组。

##### 10.Array.of()

`Array.of()` 方法创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。

##### 11.entries()、keys()、values()

ES6 提供三个新的方法 —— entries()，keys()和values() —— 用于遍历数组。它们都返回一个遍历器对象，可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历

##### ES5数组的方法

array.splice(start,deleteCount,item1,item2,...)：新增、修改、删除

start 为数组的开始坐标

deleteCount 为需要从开始坐标起，删除的个数，可以为0，代表不删除

item 为需要新增进去的元素

slice():删除、修改

str.slice(start, end) 方法可提取字符串的某个部分，并以新的字符串返回被提取的部分。

使用 start（包含） 和 end（不包含） 参数来指定字符串提取的部分。

字符串中第一个字符位置为 0, 第二个字符位置为 1, 以此类推。

**提示：** 如果是负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推。

###### Array.prototype.slice()`**slice()**` 方法返回一个新的数组对象，这一对象是一个由 `begin` 和 `end` 决定的原数组的**浅拷贝**（包括 `begin`，不包括`end`）。原始数组不会被改变。

splice():所进行的操作会影响到原数组

slice():所进行的操作不会影响到原数组

`concat()` 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

`includes()` 方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回 false。

`fill()` 方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引。

push() - 末尾添加元素   返回值为添加完后的数组的长度
unshift() - 开头添加元素 返回值是添加完后的数组的长度
pop() - 末尾删除元素，只能删一个，返回值是删除的元素
shift() - 开头删除元素，只能删除一个，返回值是删除的元素
splice(i，n) - 增删改元素  删除从i(索引值)开始之后的那个元素。返回值是删除的元素
concat() - 拼合两个数组 返回值为连接后的新数组

split()将字符串转化为数组

filter() - 过滤数组元素  返回一个满足要求的数组
forEach() - 遍历操作原数组 和for循环一样，是代替for
indexOf() - 顺序查找第一个
lastIndexOf() - 倒序查找第一个
map() - 遍历返回新数组
reverse() - 反转数组元素，返回值是反转后的数组
slice(start,end) - 查找指定位置元素 切去索引值start到索引值end的数组，不包含end索引的值，返回值是切出来的数组
sort() - 数组排序，返回值是排好的数组，默认是按照最左边的数字进行排序，不会按照数字大小排序
toString() - 数组转字符串
includes() - 数组包含某元素 indexOf()返回的是数值，而includes()返回的是布尔值 indexOf()不能判断NaN,includes()则可以判断
fill() - 填充数组 填充完后会改变原数组
reduce() - 数组累计
find() - 查找数组元素 找到第一个符合条件的数组成员
findIndex() - 查找元素索引  找到第一个符合条件的数组成员的索引值

##### 如何使用ES6语法给数组去重

1.使用set

先看一下Set定义：Set对象是值的集合，你可以按照插入的顺序迭代它的元素。Set中的元素只会出现一次，即Set中的元素是唯一的。Set对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

通过数组参数创建Set，因为Set只接受唯一值，重复的值会被去掉。创建之后，重复的值会被去掉。然后在用...操作符转换回数组。

```
const array = ['a', 1, 2, '1', 'a', 3];
const uniqueSet = new Set(array);
console.log(uniqueSet) //{"a", 1, 2, "1", 3}
const backToArray = [...uniqueSet];
console.log(backToArray) //["a", 1, 2, "1", 3]
```

##### 数组遍历方法

###### 1.for循环

使用临时变量，将长度缓存起来，避免重复获取数组长度，当数组较大时优化效果才会比较明显。

```
for(j=0,len=arr.length;j<len;j++){

}
```

###### 2.foreach循环

遍历数组中的每一项，没有返回值，对原数组没有影响，不支持IE

```
//没有返回值
arr.forEach((item,index,array)=>{
	//执行代码
})
//参数：item数组中的当前项， index当前项的索引， array原始数组
//数组中有几项，那么传递进去的匿名回调函数就需要执行几次
```

###### 3.map循环

有返回值。可以return出来

map的回调函数中支持return返回值；return的是啥，相当于把数组中的这一项变为啥（并不影响原来的数组，只是相当于把原数组克隆一份，把克隆的这一份的数组中的对应项改变了）

```
arr.map(function(value,index,array){
	//do something
	return xxx;
})
```

```
var arr = [12, 23, 24, 42, 1];
var res= arr.map(function(item,index,arr){
	return item*10;
})
console.log(res); // [120, 230, 240, 420, 20] 原数组拷贝了一份，并进行了修改
console.log(arr); //[12, 23, 24, 42, 1] 原数组并未发生变化
```

###### 4.forof遍历

可以正确响应break、continue和return语句

```
for(var value of myArray){
	console.log(value);
}
```

###### 5.filter遍历

不会改变原始数组，返回新数组

```
var arr = [
	{id: 1, text: 'aa', done: true},
	{id: 2, text: 'bb', done: false}
];
console.log(arr.filter(item=>item.done))
```

转为ES5

```
arr.filter(function(item){
	return item.done;
});
```

```
var arr = [73, 84, 56, 22, 100];
var newArr = arr.filter(item => item > 80); //得到新数组 [84, 100]
console.log(newArr, arr)
```

###### 6.every遍历

every()是对数组中的每一项运行给定函数，如果该函数对每一项返回true，则返回true。

```
var arr = [1, 2, 3, 4, 5, 6];
console.log(arr.every(function(item, index, array){
	return item > 3;
}));
//false
```

###### 7.some遍历

some()是对数组中每一项运行指定函数，如果该函数对任一项返回true,则返回true。

```
var arr = [1, 2, 3, 4, 5, 6];
console.log(arr.some(function(item, index, array){
	return item > 3;
}));
//true
```

###### 8.reduce

reduce()方法接收一个函数作为累加器(accumulator)，数组中的每个值（从左到右）开始缩减，最终为一个值。

reduce接受一个函数，函数有四个参数，分别是：上一次的值，当前值，当前值的索引，数组

```
[0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, index, array){
	return previousValue + currentValue;
});
```

```
var total = [1,2,3,4].reduce((a, b)=>a+b);
//10
```

![img](https://images2018.cnblogs.com/blog/1403464/201807/1403464-20180713112335425-223204218.png)

reduce还有第二个参数，我们可以把这个参数作为第一次调用callback时的第一个参数，上面这个例子因为没有第二个参数，所以直接从数组的第二项开始，如果我们给了第二个参数为5，那么结果就是这样的：

```
[0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, index, array){
 return previousValue + currentValue;
},5);
```

![img](https://images2018.cnblogs.com/blog/1403464/201807/1403464-20180713112406091-1932166695.png)

第一次调用的previousValue的值就用传入的第二个参数代替。

###### 9.reduceRight

reduceRight()方法的功能和reduce()功能是一样的，不同的是reduceRight()从数组的末尾向前将数组中的数组项做累加。

reduceRight()首次调用回调函数callbackfn时，prevValue和curValue可以是两个值之一。如果调用reduceRight()时提供了initialValue参数，则prevValue等于initialValue，curValue等于数组中的最后一个值。如果没有提供initialValue参数，则prevValue等于数组最后一个值，curValue等于数组中倒数第二个值。

```
var arr = [0,1,2,3,4];
 
arr.reduceRight(function (preValue,curValue,index,array) {
    return preValue + curValue;
}); // 10
```

###### 10.find

find()方法返回数组中符合测试函数条件的第一个元素。否则返回undefined。

```
var stu = [
    {
        name: '张三',
        gender: '男',
        age: 20
    },
    {
        name: '王小毛',
        gender: '男',
        age: 20
    },
    {
        name: '李四',
        gender: '男',
        age: 20
    }
]
function getStu(element){
   return element.name == '李四'
}
 
stu.find(getStu)
//返回结果为
//{name: "李四", gender: "男", age: 20}
```

ES6方法为：

```
stu.find((element) => (element.name == '李四'))
```

###### 11.findIndex

对于数组中的每个元素，findIndex方法都会调用一次回调函数（采用圣墟索引顺序），直到有元素返回true。只要有一个元素返回true，findIndex立即返回该返回true的元素的索引值。如果数组中没有任何元素返回true，则findIndex返回-1。

findIndex不会改变数组对象。

```
[1, 2, 3].findIndex(x => x==2 ) //1
[1, 2, 3].findIndex(x => x ==4) //-1
```

###### 12.keys，values，entries

ES6提供三个新的方法---entries()、keys()和values()---用于遍历数组。它们都返回一个遍历器对象，可以用for...of循环遍历，唯一的区别是key()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历

```
for (let index of ['a', 'b'].keys()) {
console.log(index);
}
// 0
// 1
for (let elem of ['a', 'b'].values()) {
console.log(elem);
}
// 'a'
// 'b'
for (let [index, elem] of ['a', 'b'].entries()) {
console.log(index, elem);
}
// 0 "a"
// 1 "b"
```



把Set转换为数组还可以使用Array.from，如下：

```
const array = ['a', 1, 2, '1', 'a', 3];
const backToArray = Array.from(new Set(array));
console.log(backToArray)//["a", 1, 2, "1", 3]
```

2.使用Filter

filter:根据条件过滤元素，返回符合条件的新数组。如果条件返回true，则元素包含在新数组中；如果条件返回false，则新数组中不包含这个元素。

```
const array = ['a', 1, 2, 'a', 3, '1'];
const arr = [];
array.filter((item,index)=>{
	console.log(item,
	index,
	array.indexOf(item),
	array.indexOf(item) === index
	)
	return array.indexOf(item) === index ? arr.push(item) : '';
})
console.log(arr) // ["a", 1, 2, 3, "1"]

```

也可以获取重复的元素：

```
const array = ['a', 1, 2, 'a', 3, '1'];
const arr = [];
array.filter((item,index)=>{
	return array.indexOf(item) !== index ? arr.push(item) : '';
})
console.log(arr) //["a"]
```

3.使用Reduce

通过reducer函数，符合条件就把元素添加到结果数组中，不用额外创建数组变量。

```
const array = ['a', 1, 2, 'a', 3, '1'];
array.reduce((unique,item)=>{
	return unique.includes(item) ? unique : [...unique, item]
},[]);
```


