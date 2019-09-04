call、apply、bind的介绍

##### 语法：

fun.call(thisArg, param1,  param2,...)

fun.apply(thisArg, [param1, param2,...])

fun.bind(thisArg, param1, param2,...)

##### 返回值：

call/apply：fun执行的结果

bind：返回fun的拷贝，并拥有指定的this值和初始参数。不调用函数，但生成一个函数并指定函数内的this。返回值是一个函数。

##### 参数：

thisArg(可选)：

1. fun的this指向thisArg对象
2. 非严格模式下：thisArg为空、null、undefined，fun中的this指向window对象
3. 严格模式下：fun的this为undefined
4. 值为原始值（数字、字符串、布尔值）的this会指向该原始值的自动包装对象，如String、Number、Boolean

param1,param2(可选)：传给fun的参数

1. 如果param不传或为null/undefined，则表示不需要传入任何参数
2. apply第二个参数为数组，数组内的值为传给fun的参数

##### 调用call/ apply/ bind的必须是个函数

call 、apply和bind是挂在Function对象上的三个方法，只有函数才有这些方法

只要是函数就可以，比如：Object.prototype.toString就是个函数，我们经常看到这样的用法：Object.prototype.toString.call(data)

##### 作用：

1. 改变函数执行时的this指向
2. 借用其他对象身上的方法
3. 继承

##### 如何不弄混call和apply

apply是以a开头，它传给fun的参数是Array,也是以a开头的

##### 区别：

###### call与apply的唯一区别：

传给fun的参数写法不同：

- apply是第2个参数，这个参数是一个数组：传给fun参数都写在数组中。
- call从第2~n的参数都是传给fun的。

###### call/apply与bind的区别

执行：

- call/apply改变了函数的this上下文后马上执行该函数
- bind则是返回改变了上下文后的函数，不执行该函数

返回值：

- call/apply返回fun的执行结果
- bind返回fun的拷贝，并指定了fun的this指向，保存了fun的参数

性能：

apply的性能比call的性能低，低5到6倍左右

##### call/apply/bind的核心理念：借用方法

借助已实现的方法，改变方法中数据的this指向，减少重复代码，节省内存。

```
const result = Math.max.apply(null,[3,5,9]);
```

Math.max就是借用的方法。

##### call、apply，该用哪个？

call，apply的效果完全一样，它们的区别在于：

- 参数数量/顺序确定就用call，参数数量/顺序不确定的话就用apply。
- 考虑可读性：参数数量不多就用call，参数数量比较多的话，把参数整合成数组，使用apply。
- 参数集合已经是一个数组的情况，用apply,

###### 举例借用其他对象身上的方法

const str='hehehaha';

let arr = Array.prototype.slice.call(str);

console.log(arr) // ["h", "e", "h", "e", "h", "a", "h", "a"]

不是所有的方法都可以借用：

const str='hehehaha';

console.log([].push.call(str)) //VM211:2 Uncaught TypeError: Cannot assign to read only property 'length' of object '[object String]'

报错的原因：字符串的length属性不可写。是只读的。
