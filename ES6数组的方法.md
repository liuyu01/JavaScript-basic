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
