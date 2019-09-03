##### 1.reduce  递归累加

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

map最适合做的事是映射，生成原始数据的特征信息的map

相比较之下，forEach是没有返回值的，即便加上return也没有用

w = potatoes.forEach(potato =>  {return potato.weight += 20 })

console.log(w) //undefined

而map有返回值，可以把土豆的重量统计汇总，整理一份表格

w = potatoes.map(potato => {return potato.weight +=20 })

console.log(w) //[ 70, 100, 140, 60, 130, 80 ]

##### 4.filter 过滤筛选

var bigOnes = potatoes.filter(potato =>{return potato.weight > 100 })

console.log(bigOnes)//[ { id: '1003', weight: 120 }, { id: '1005', weight: 110 } ]

返回一个新的对象数组，并不会改变原数组

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

push() - 末尾添加元素
unshift() - 开头添加元素
pop() - 末尾删除元素
shift() - 开头删除元素
splice() - 增删改元素
concat() - 拼合两个数组
filter() - 过滤数组元素
forEach() - 遍历操作原数组
indexOf() - 顺序查找第一个
lastIndexOf() - 倒序查找第一个
map() - 遍历返回新数组
reverse() - 反转数组元素
slice() - 查找指定位置元素
sort() - 数组排序
toString() - 数组转字符串
includes() - 数组包含某元素
fill() - 填充数组
reduce() - 数组累计
find() - 查找数组元素
findIndex() - 查找元素索引
