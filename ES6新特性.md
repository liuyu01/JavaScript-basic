ES全称ECMAScript,ECMAScript是ECMA制定的标准化脚本语言。目前JavaScript使用的ECMAScript版本为ECMA-417。

ECMA规范最终由TC39敲定。TC39由包括浏览器厂商在内的各方组成，他们开会推动JavaScript提案沿着一条严格的发展道路前进。从提案到入选ECMA规范主要有以下几个阶段：

Stage 0: strawman——最初想法的提交。

Stage 1: proposal（提案）——由TC39至少一名成员倡导的正式提案文件，该文件包括API事例。

Stage 2: draft（草案）——功能规范的初始版本，该版本包含功能规范的两个实验实现。

Stage 3: candidate（候选）——提案规范通过审查并从厂商那里收集反馈

Stage 4: finished（完成）——提案准备加入ECMAScript，但是到浏览器或者Nodejs中可能需要更长的时间。

##### ES6新特性（2015）

常用的：

- 类(class)
- 模块化(Module) export 导出 import 导入
- 箭头函数
- 函数参数默认值
- 模板字符串
- 解构赋值
- 扩展操作符
- 对象属性简写
- Promise
- let const const与let都是块级作用域

ES7新特性（2016）

- 数组includes()方法，用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回true，否则返回false。
- a ** b指数运算法，它与Math.pow(a,b)相同。

##### ES6中对象新增的方法

Object.is() 目的是为了保证在所有环境中，只要两个值是一样的，它们就应该相等，其行为与===基本一致，用来比较两个值是否严格相等。有两个不同之处：一是+0不等于-0而是NaN等于自身。

Object.assign()用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）上。

Object.getOwnPropertyDescriptors() 返回指定对象所有自身属性(非继承属性)的描述对象。

Object.getPrototypeOf()用于读取一个对象的原型对象

Object.setPrototypeOf()用来设置一个对象的prototype对象，返回参数对象本身。它是ES6正式推荐的设置原型对象的方法。

Object.keys() 返回一个成员是参数对象自身的（不含继承的）所有可遍历(enumerable)属性的键名的数组

Object.values()返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历(enumerable)属性的键值。

Object.entries()返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历(enumerable)属性的键值对数组。

Object.fromEntries()是Object.entries()的逆操作，用于将一个键值对数组转为对象。
