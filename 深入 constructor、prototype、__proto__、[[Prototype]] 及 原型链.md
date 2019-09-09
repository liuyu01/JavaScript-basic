##### 对象

在JS中，万物皆对象，对象又分为普通对象和函数对象，其中Object 、Function为JS自带的函数对象。

凡是通过new Function()创建的对象都是函数对象。

##### 构造函数

```
function Foo(name,age){
    this.name = name;
    this.age = age;
    this.class = 'class';
}
let f = new Foo('aa',20);
```

每一个实例都有一个constructor(构造函数)属性，该属性指向对象本身。

```
f.constructor === Foo; //true
```

构造函数和普通函数的区别在于，使用new生成实例的函数就是构造函数，直接调用的就是普通函数。

JS本身不提供一个class实现。（在ES2015/ES6中引入了class关键字，但只是语法糖，JavaScript仍然是基于原型的）。

##### 构造函数扩展

- let a = {}其实是let a = new Object()的语法糖
- let a =  []其实是let a = new Array()的语法糖
- function Foo(){}其实是var Foo = new Function(){}的语法糖
- 可以使用instanceof判断一个函数是否为一个变量的构造函数

##### Symbol是构造函数吗？

Symbol是基本数据类型，它并不是构造函数，因为它不支持new Symbol()语法。直接使用Symbol()即可。

Symbol()返回的symbol值是唯一的。

可以通过Symbol.for(key)获取全局唯一的symbol

##### constructor的值是只读的吗？

对于引用类型来说constructor属性值是可以修改的，但是对于基本类型来说是只读的。

因为：原生构造函数式只读的。

注意：null 和 undefined 是没有constructor属性的。

##### 原型

在JS中，每个对象都有自己的原型。当我们访问对象的属性和方法时，JS会先访问对象本身的方法和属性。如果对象本身不包含这些属性和方法，则访问对象对应的原型。

##### prototype

所有函数都有一个prototype（显示原型）属性，属性值是一个普通的对象。对象以其原型为模板，从原型继承方法和属性，这些属性和方法定义在对象的构造函数的prototype属性上，而非对象实例本身。

![img](https://user-gold-cdn.xitu.io/2019/8/25/16cc936f3b4214f4?imageslim)

##### ____ proto ___

每个实例对象（object）都有一个隐式原型属性（称之为__ proto __ )指向了创建该对象的构造函数的原型。也就是指向了函数的prototype属性。已被弃用

当 `new Foo()` 时，`__proto__` 被自动创建。并且

```
foo.__proto__ === Foo.prototype; // true
```

 ![img](https://user-gold-cdn.xitu.io/2019/8/25/16cc936fa1489e94?imageslim)

##### [[Prototype]]

[[Prototype]]是对象的一个内部属性，外部代码无法直接访问。

遵循ECNAScript标准，someObject.[[Prototype]]符号用于指向someObject的原型

##### new的实现过程

- 新生成了一个对象
- 链接到原型
- 绑定this
- 返回新对象

根据定义，`null`没有原型，并作为这个**原型链** 中的最后一个环节。

##### 原型链

每个对象拥有一个原型对象，通过 __ proto __ 指针指向上一个原型，并从中继承方法和属性，同时原型对象也可能拥有原型，这样一层一层，最终指向null，这种关系被称为原型链。根据定义，null没有原型，并作为这个原型链中的最后一个环节。

原型链的基本思想是利用原型，让一个引用类型继承另一个引用类型的属性及方法。

##### 总结 

- Symbol是基本数据类型，并不是构造函数，因为它不支持语法new Symbol()，但其原型上有constructor属性，即Symbol.prototype.constructor
- 引用类型constructor是可以修改的，但对于基本类型来说它是只读的，null和undefined 没有constructor属性。
- __ proto __ 是每个实例对象都有的属性，prototype是其构造函数的属性，在实例上并不存在，所以这两个并不一样，但foo.__ proto __和Foo.prototype指向同一个对象。
- __ proto __ 属性在ES6时被标准化，但因为性能问题并不推荐使用，推荐使用Object.getPrototypeof()。
- 每个对象拥有一个原型对象，通过 `__proto__` 指针指向上一个原型 ，并从中继承方法和属性，同时原型对象也可能拥有原型，这样一层一层向上，最终指向 `null`，这就是原型链。
- 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的原型中寻找，以及该对象的原型的原型，一层一层向上查找，直到找到一个名字匹配的属性 / 方法或到达原型链的末尾（`null`）

##### 构造函数、原型对象以及实例对象之间的关系

1.构造函数（constructor）中有prototype属性，它是一个指针，指向原型对象

2.原型对象中有constructor属性，是一个指针，指向该原型对应的构造函数

3.实例（instance）内部有__ proto __属性，是一个指针，指向该构造函数的原型

![img](https://user-gold-cdn.xitu.io/2019/8/30/16ce08c58e001f74?imageslim)



```
实例.__proto__ === 原型
原型.constructor === 构造函数
构造函数.prototype === 原型
```

##### 原型链

属性查找机制：当查找对象的属性时，如果实例对象自身不存在该属性，则沿着原型链往上一级查找，找到时则输出，不存在时，则继续沿着原型链往上一级查找，直到最顶级的原型对象Object.prototype，如果是没找到，则输出undefined。

属性修改机制：只会修改实例对象本身的属性，如果不存在，则进行添加该属性，如果需要修改原型的属性时，则可以用：b.prototype.x = 2;但是这样会造成所有继承于该对象的实例的属性发生改变。





