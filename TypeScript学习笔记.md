##### TypeScript笔记

TypeScript是JavaScript的一个超集，支持ECMAScript6标准。

TypeScript由微软开发的自由和开源的编程语言。

TypeScript设计目标是开发大型应用，它可以编译成纯JavaScript，编译出来的JavaScript可以运行在任何浏览器上。

###### 语言特性

TypeScript是一种给JavaScript添加特性的语言扩展。增加的功能包括：

- 类型批注和编译时类型检查
- 类型推断
- 类型擦除
- 接口
- 枚举
- Mixin
- 泛型编程
- 名字空间
- 元祖
- Await

以下功能是从ECMAScript2015反向移植而来：

- 类
- 模块
- lambda函数的箭头语法
- 可选参数以及默认参数

###### JavaScript与TypeScript的区别

TypeScript是JavaScript的超集，扩展了JavaScript的语法，因此现有的JavaScript代码可与TypeScript一起工作无需任何修改，TypeScript通过类型注解提供编译时的静态类型检查。

TypeScript可处理已有的JavaScript代码，并只对其中的TypeScript代码进行编译。

通常我们使用.ts作为TypeScript代码文件的扩展名。

TypeScript转换为JavaScript过程如下图：

![img](https://www.runoob.com/wp-content/uploads/2019/01/typescript_compiler.png)

sourcemap是一个存储源代码与编译代码对应位置映射的信息文件。

###### 空白和换行

TypeScript会忽略程序中出现的空格、制表符和换行符。

空格、制表符通常用来缩进代码，使代码易于阅读和理解。

###### TypeScript区分大小写

TypeScript区分大写和小写字符。

###### 分号是可选的

每行指令都是一段语句，你可以使用分号或不使用，分号在TypeScript中是可选的，建议使用。

###### TypeScript注释

只是是一个良好的习惯，虽然很多程序员讨厌注释，但还是建议你在每段代码写上文字说明。

注释可以提高程序的可读性。

注释可以包含有关程序一些信息，如代码的作者，有关函数的说明等。

编译器会忽略注释。

###### TypeScript支持两种类型的注释

- 单行注释（//） 在//后面的文字都是注释内容。
- 多行注释（/* */）这种注释可以跨越多行。

###### TypeScript与面向对象

面向对象是一种现实世界理解和抽象的方法。

TypeScript是一种面向对象的编程语言。

面向对象主要有两个概念：对象和类。

- 对象：对象是类的一个实例，有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
- 类：类是一个模板，它描述一类对象的行为和状态。
- 方法：方法是类的操作的实现步骤。

###### TypeScript基础类型

| 数据类型   | 关键字    | 描述                                                         |
| ---------- | --------- | ------------------------------------------------------------ |
| 任意类型   | any       | 声明为any的变量可以赋予任意类型的值。                        |
| 数字类型   | number    | 双精度64位浮点值。它可以用来表示整数和分数。                 |
| 字符串类型 | string    | 一个字符系列，使用单引号(')或双引号(")来表示字符串类型。反引号(`)来定义多行文本和内嵌表达式。 |
| 布尔类型   | boolean   | 表示逻辑值：true和false。                                    |
| 数组类型   | 无        | 声明变量为数组。 在元素类型后面加上[]  let arr: number[] = [1,2];                                           或者使用数组泛型 let arr: Array<number> = [1, 2]; |
| 元组       | 无        | 元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。    let x: [string, number];   x=['Runoob', 1];//运行正常  x=[1, 'Runoob'];//报错  console.log(x[0]);//输出Runoob |
| 枚举       | enum      | 枚举类型用于定义数值集合。                                   |
| void       | void      | 用于标识方法返回值的类型，表示该方法没有返回值。             |
| null       | null      | 表示对象值缺失。                                             |
| undefined  | undefined | 用于初始化变量为一个未定义的值                               |
| never      | never     | never是其它类型（包括null和undefined）的子类型，代表从不会出现的值。 |

注意：TypeScript和JavaScript没有整数类型。

Any类型

任意值是TypeScript针对编程时类型不明确的变量使用的一种数据类型。

Null 和 Undefined

null 

在JavaScript中null表示“什么都没有”。

null是一个只有一个值的特殊类型。表示一个空对象引用。

用typeof检测null返回是object。

undefined

在 JavaScript 中, undefined 是一个没有设置值的变量。

typeof 一个没有值的变量会返回 undefined。

Null 和 Undefined 是其他任何类型（包括 void）的子类型，可以赋值给其它类型，如数字类型，此时，赋值后的类型会变成 null 或 undefined。而在TypeScript中启用严格的空校验（--strictNullChecks）特性，就可以使得null 和 undefined 只能被赋值给 void 或本身对应的类型，

如果一个类型可能出现 null 或 undefined， 可以用 | 来支持多种类型，

let x: number | null | undefined;

never类型

never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。这意味着声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环），

###### TypeScript变量声明

变量是一种使用方便的占位符，用于引用计算机内存地址。

TypeScript变量的命名规则：

- 变量名称可以包含数字和字母
- 除了下划线_和美元$符号外，不能包含其他特殊字符，包括空格
- 变量名不能以数字开头

声明变量的方式：

1）声明变量的类型及初始值

var [变量名] ：[类型] = 值；var uname:string = "Runoob";

2）声明变量的类型，但是没有初始值，变量值会设置为undefined;

var [变量名] : [类型]；var uname:string;

3）声明变量并初始值，但不设置类型，该变量可以是任意类型；

var [变量名] = 值； var uname = "Runoob";

4）声明变量没有设置类型和初始值，类型可以是任意类型，默认初始值为undefined;

var [变量名]； var uname;

###### 类型断言

类型断言可以用来手动指定一个值得类型，即允许变量从一种类型变更为另一种类型。

语法格式为：

```
<类型>值 或 值 as 类型
```

###### 类型推断

当类型没有给出时，TypeScript编译器利用类型推断来推断类型。如果由于缺乏声明而不能让推断出类型，那么它的类型被视为默认的动态any 类型。

###### 变量作用域

变量作用域指定了变量定义的位置。

程序中变量的可用性由变量作用域决定。

TypeScript有以下几种作用域：

- 全局作用域 --- 全局变量定义在程序结构的外部，它可以在你代码的任何位置使用
- 类作用域 --- 这个变量也可以称为字段。来变量声明在一个类里头，但在类的方法外面。该变量可以通过类的对象类访问。类变量也可以是静态的，静态的变量可以通过类名直接访问。
- 局部作用域 --- 局部变量，局部变量只能在声明它的一个代码块（如：方法）中使用。

###### TypeScript运算符

TypeScript主要包括以下几种运算：

- 算数运算符  +(加法)、-(减法)、*(乘法)、/(除法)、%(取模（余数）)、++(自增)、--(自减)
- 逻辑运算符  && 、||、!
- 关系运算符 ==(等于)、!=(不等于)、>(大于)、<(小于)、>=(大于或等于)、<=(小于或等于)
- 按位运算符 & 、| 、~、^、<<、>>、>>>
- 赋值运算符 =(赋值)、+=(先进行加运算后赋值)、-=(先进行减运算后赋值)、*=(先进行乘运算后赋值)、/=(先进行除运算后赋值)
- 三元/条件运算符 Test ? expr1 : expr2
- 字符串运算符 连接运算符(+)、负号运算符(-)
- 类型运算符 typeof 、instanceof

###### TypeScript条件语句

TypeScript条件语句是通过一条或多条语句的执行结果（True或False）类决定执行的代码块。

条件语句：

- if语句
- if...else语句
- if...else if...else语句
- switch 语句

###### TypeScript循环

循环允许我们多次执行一个语句或语句组

for循环

for...in循环：用于一组值的集合或列表进行迭代输出。

for...of

forEach

every

some

while循环：while语句在给定条件为true时，重复执行语句或语句组。循环主体执行之前会先测试条件。

do...while循环：不像for和while循环，它们是在循环头部测试循环条件。do...while循环是在循环的尾部检查它的条件。

break语句

break语句有以下两种用法：

1.当break语句出现在一个循环内时，循环会立即终止，且程序流将继续执行紧接着循环的下一跳语句。

2.它可用于终止switch语句中的一个case.

continue语句

continue语句有点像break语句。但它不是强制终止，continue会跳过当前循环中的代码，强迫开始下一次循环。

对于for循环，continue语句执行后自增语句仍然会执行。对于while和do...while循环，continue语句重新执行条件判断语句。

###### TypeScript函数

函数是一组一起执行一个任务的语句。

函数声明告诉编译器函数的名称、返回类型和参数。函数定义提供了函数的实际主体。

函数返回值

```
function function_name():return_type{
	//语句
	return value;
}
```

- return_type 是返回值的类型
- return关键词后跟着要返回的结果
- 一个函数只能有一个return语句
- 返回值的类型需要与函数定义的返回类型(return_type)一致

```
function greet():string{
	return 'Hello World';
}
```

带参数函数

语法格式如下：

```
function func_name(param1[:=datatype], param2[:datatype]){
	
}
param1、param2为参数名
datatype为参数类型
```

```
function add(x: number, y:number):number{
	return x + y;
}
```

可选参数和默认参数

可选参数：在TypeScript函数里，如果我们定义了参数，则必须传入这些参数，除非将这些参数设置为可选，可选参数使用问号标识?

```
function buildName(firstName: string, lastName?: string){
	if(lastName)
		return firstName + " " + lastName;
	else 
		return firstName;
}

```

可选参数必须跟在必需参数后面。

默认参数：也可以设置参数的默认值，这样在调用函数的时候，如果不传入该参数的值，则使用默认参数，语法格式为：

```
function func_name(param1[:type], param2[:type] = default_value){

}
```

参数不能同时设置为可选和默认。

剩余参数

有一种情况，我们不知道要向函数传入多少个参数，这时候我们就可以使用剩余参数来定义。

剩余参数语法允许我们将一个不确定数量的参数作为一个数组传入。

```
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}
```

匿名函数：是一个没有函数名的函数

构造函数

递归函数：即在函数内调用函数本身

Lambda函数：也称之为箭头函数

函数重载：

重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。

每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。

###### TypeScript Number

TypeScript与JavaScript类似，支持Number对象。

Number对象是原始数值的包装对象。

语法：

```
var num = new Number(value);
```

注意：如果一个参数值不能转换为一个数字将返回NaN(非数字值)。

Number对象属性

MAX_VALUE:可表示的最大的数，MAX_VALUE属性值接近于1.79E+308。大于MAX_VALUE的值代表’Infinity‘。

MIN_VALUE:可表示的最小的数，即最接近0的正数（实际上不会变成0）。最大的负数是-MIN_VALUE,MIN_VALUE的值约为5e-324，小于MIN_VALUE('underflow values')的值将会转换为0。

NaN:非数字值（Not-A-Number）

NEGATIVE_INFINITY:负无穷大，溢出时返回该值。该值小于MIN_VALUE。

POSITIVE_INFINITY:正无穷大，溢出时返回该值。该值大于MAX_VALUE。

prototype:Number对象的静态属性。使您有能力向对象添加属性和方法。

constructor:返回对创建此对象的Number函数的引用。

Number对象方法：

toExponential():把独享的值转换为指数计数法

toFixed():把数字转换为字符串，并对小数点指定位数。

toLocaleString():把数字转换为字符串，使用本地数字格式顺序。

toPrecision():把数字格式化为指定的长度。

toString():把数字转换为字符串，使用指定的基数。数字的基数是2~36之间的整数。若省略该参数，则使用基数10。

valueOf()：返回一个Number对象的原始数字值。

###### TypeScript String(字符串)

String对象用于处理文本（字符串）。

String对象属性

constructor : 对创建该对象的函数的引用。

length : 返回字符串的长度

prototype : 允许您向对象添加属性和方法

String方法：

charAt() : 返回指定位置的字符。

charCodeAt() : 返回在指定的位置的字符的Unicode编码。

 concat() : 连接两个或更多字符串，并返回新的字符串。

indexOf() :返回某个指定的字符串值在字符串中首次出现的位置。

lastIndexOf() :从后向前搜索字符串，并从起始位置（0）开始计算返回字符串最后出现的位置。

localeCompare() : 用本地特定的顺序来比较两个字符串。

match() : 查找找到一个或多个正则表达式的匹配。

replace() : 替换与正则表达式匹配的子串。

search() : 检索与正则表达式相匹配的值。

slice() :提取字符串的片断，并在新的字符串中返回被提取的部分。

split() : 把字符串分割为子字符串数组。

substr() :从起始索引号提取字符串中指定数目的字符。

substring() :提取字符串中两个指定的索引号之间的字符。

toLocaleLowerCase():根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射。

toLocaleUpperCase() : 根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射。

toLowerCase() : 把字符串转换为小写。

toUpperCase() : 把字符串转换为大写。

toString() :返回字符串。

valueOf(): 返回指定字符串对象的原始值。

###### TypeScript Array(数组)

数组对象是使用单独的变量名来存储一系列的值。

TypeScript声明数组的语法格式如下：

```
var array_name[:datatype];
array_name=[val1, val2, valn...];
```

或者直接在声明时初始化：

```
var array_name:[data type]=[val1,val2...valn];
```

如果数组声明时未设置类型，则会被认为是any类型，在初始化时根据第一个元素的类型来推断数组的类型。

创建一个number类型的数组：

```
var numlist:number[] = [2,3,4,5];
```

Array对象

Array对象的构造函数接受以下两种值：

1）表示数组大小的数值

2）初始化的数组列表，元素使用逗号分隔值

多维数组

一个数组的元素可以是另外一个数组，这样就构成了多维数组（Multi-dimensional Array）。

最简单的多维数组是二维数组，定义方式如下：

```
var arr_name:datatype[][]=[ [val1,val2,val3],[v1,v2,v3] ]
```

数组方法：

concat() : 连接两个或更多的数组，并返回结果。

every() : 检测数值元素的每个元素是否都符合条件。

filter() : 检测数值元素，并返回符合条件所有元素的数组。

forEach() : 数组每个元素都执行一次回调函数。

indexOf() :搜索数组中的元素，并返回它所在的位置。如果搜索不到，返回值-1，代表没有此项。

join() :把数组的所有元素放入一个字符串中。

lastIndexOf() : 返回一个指定的字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索。

map() : 通过指定函数处理数组的每个元素，并返回处理后的数组。

pop() : 删除数组的最后一个元素并返回删除的元素。

push() : 向数组的末尾添加一个或更多元素，并返回新的长度。

reduce() : 将数组元素计算为一个值（从左到右）。

reduceRight() : 将数组元素计算为一个值（从右到左）。

reverse() :反转数组的元素顺序。

shift() : 删除并返回数组的第一个元素。

slice() :选取数组的一部分，并返回一个新数组。

some() :检测数组元素中是否有元素符合指定条件。

sort() : 对数组的元素进行排序。

splice() : 从数组中添加或删除元素。

toString() : 把数组转换为字符串，并返回结果。

unshift() :向数组的开头添加一个或更多元素，并返回新的长度。

###### TypeScript Map对象

Map对象保存键值对，并且能够记住键的原始插入顺序。

任何值（对象或者原始值）都可以作为一个键或一个值。

Map是ES6中引入的一种新的数据结构。

创建Map

TypeScript使用Map类型和new关键字来创建Map:

```
let myMap = new Map();
```

初始化Map,可以以数组的格式来传入键值对：

```
let myMap = new map([
	["key1","value1"],
	["key2","value2"]
]);
```

Map相关的函数与属性：

- map.clear()  ---移除Map对象的所有键/值对。
- map.set() --- 设置键值对，返回该Map对象。
- map.get() --- 返回键对应的值，如果不存在，则返回undefined。
- map.has() --- 返回一个布尔值，用于判断Map中是否包含键对应的值。
- map.delete() --- 删除Map中的元素，删除成功返回true，失败返回false。
- map.size ---返回Map独享键/值对的数量。
- map.keys() --- 返回一个Iterator对象，包含了Map对象中每个元素的键。
- map.values() --- 返回一个新的Iterator对象，包含了Map对象中每个元素的值。

迭代Map

Map对象中的元素是按顺序插入的，我们可以迭代Map对象，每一次迭代返回[key, value]数组。

TypeScript使用for...of来实现迭代。

###### TypeScript元组

我们知道数组中元素的数据类型都一般是相同的，如果存储的元素数据类型不同，则需要使用元组。

元组中允许存储不同类型的元素，元组可以作为参数传递给函数。

创建元组的语法格式如下：

```
var tuple_name = [value1, value2, value3,...valuen];
```

声明一个元组并初始化：

```
var mytuple = [10, 'Runoob'];
```

访问元组

元组中元素使用索引来访问，第一个元素的索引值为0，第二个为1，依次类推第n个为n-1,语法格式如下：

```
tuple_name[index]
```

元组运算

可以使用以下两个函数向元组添加新元素或者删除元素：

- push()向元组添加元素，添加在最后面
- pop()从元组中移除元素（最后一个），并返回移除的元素

###### TypeScript联合类型

联合类型（Union Types）可以通过管道（|）将变量设置多种类型，赋值时可以根据设置的类型来赋值。

注意：只能赋值指定的类型，如果赋值其他类型就会报错。

创建联合类型的语法格式如下：

```
Type1 | Type2 | type3
```

声明一个联合类型：

```
var val:string|number;
val=12;
console.log('数字为 '+val)
val='Runoob';
console.log('字符串为 '+val)
```

联合类型数组

可以将数组声明为联合类型

var arr:number[]|string[];

###### TypeScript接口

接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。

TypeScript 接口定义如下：

```
interface interface_name{

}
```

一下实例中，我们定义了一个接口IPerson，接着定义了一个变量customer,它的类型是IPerson。

```
interface IPerson{
	firstName:string;
	lastName:string;
	sayHi：()=>string
}
var customer:IPerson={
	firstName:'Tom',
	lastName:'Hanks',
	sayHi:():string=>{return 'Hi there'}
}
```

需要注意接口不能转换为JavaScript。它只是TypeScript的一部分。

编译以上代码，得到以下JavaScript代码：

```
var customer = {
    firstName: "Tom",
    lastName: "Hanks",
    sayHi: function () { return "Hi there"; }
};
```

接口继承

接口继承就是说接口可以通过其他接口来扩展自己。

TypeScript允许接口继承多个接口。继承使用关键字extends 。

单接口继承语法格式：

```
Child_interface_name extends super_interface_name
```

多接口继承语法格式：

```
Child_interface_name extends super_interface1_name, super_interface2_name, ...,super_interfaceN_name
```

继承的各个接口使用逗号,分隔。

###### TypeScript类

TypeScript是面向对象的JavaScript。

类描述了所创建的对象共同的属性和方法。

TypeScript支持面向对象的所有特性，比如类、接口等。

TypeScript类定义方式如下：

```
class class_name {
	//类作用域
}
```

定义类的关键字为 class，后面紧跟类名，类可以包含以下几个模块（类的数据成员）：

- **字段** − 字段是类里面声明的变量。字段表示对象的有关数据。
- **构造函数** − 类实例化时调用，可以为类的对象分配内存。
- **方法** − 方法为对象要执行的操作。

创建实例化对象

使用new关键字来实例化类的对象，语法格式如下：

```
var object_name = new class_name([ arguments ])
```

类中的字段属性和方法可以使用.号来访问

类的继承

TypeScript 支持继承类，即我们可以在创建类的时候继承一个已存在的类，这个已存在的类称为父类，继承它的类称为子类。

类继承使用关键字 **extends**，子类除了不能继承父类的私有成员(方法和属性)和构造函数，其他的都可以继承。

TypeScript 一次只能继承一个类，不支持继承多个类，但 TypeScript 支持多重继承（A 继承 B，B 继承 C）。

语法格式如下：

```
class child_class_name extends parent_class_name
```

继承类的方法重写

类继承后，子类可以对父类的方法重新定义，这个过程称之为方法的重写。

其中 super 关键字是对父类的直接引用，该关键字可以引用父类的属性和方法。

static 关键字

static 关键字用于定义类的数据成员（属性和方法）为静态的，静态成员可以直接通过类名调用。

instanceof 运算符

instanceof 运算符用于判断对象是否是指定的类型，如果是返回 true，否则返回 false。

访问控制修饰符

TypeScript 中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。TypeScript 支持 3 种不同的访问权限。

- **public（默认）** : 公有，可以在任何地方被访问。
- **protected** : 受保护，可以被其自身以及其子类和父类访问。
- **private** : 私有，只能被其定义所在的类访问。

###### TypeScript对象

对象是包含一组键值对的实例。值可以是标量、函数、数组、对象等。

鸭子类型(Duck Typing)

鸭子类型（英语：duck typing）是动态类型的一种风格，是多态(polymorphism)的一种形式。

在这种风格中，一个对象有效的语义，不是由继承自特定的类或实现特定的接口，而是由"当前方法和属性的集合"决定。

> 可以这样表述：
>
> "当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。"

在鸭子类型中，关注点在于对象的行为，能作什么；而不是关注对象所属的类型。例如，在不使用鸭子类型的语言中，我们可以编写一个函数，它接受一个类型为"鸭子"的对象，并调用它的"走"和"叫"方法。在使用鸭子类型的语言中，这样的一个函数可以接受一个任意类型的对象，并调用它的"走"和"叫"方法。如果这些需要被调用的方法不存在，那么将引发一个运行时错误。任何拥有这样的正确的"走"和"叫"方法的对象都可被函数接受的这种行为引出了以上表述，这种决定类型的方式因此得名。

###### TypeScript命名空间

命名空间一个最明确的目的就是解决重名问题。

命名空间定义了标识符的可见范围，一个标识符可在多个名字空间中定义，它在不同名字空间中的含义是互不相干的。这样，在一个新的名字空间中可定义任何标识符，它们不会与任何已有的标识符发生冲突，因为已有的定义都处于其他名字空间中。

TypeScript 中命名空间使用 **namespace** 来定义，语法格式如下：

```
namespace SomeNameSpaceName { 
   export interface ISomeInterfaceName {      }  
   export class SomeClassName {      }  
}
```

以上定义了一个命名空间 SomeNameSpaceName，如果我们需要在外部可以调用 SomeNameSpaceName 中的类和接口，则需要在类和接口添加 **export** 关键字。

要在另外一个命名空间调用语法格式为：

```
SomeNameSpaceName.SomeClassName;
```

如果一个命名空间在一个单独的 TypeScript 文件中，则应使用三斜杠 /// 引用它，语法格式如下：

```
/// <reference path = "SomeFileName.ts" />
```

嵌套命名空间

命名空间支持嵌套，即你可以将命名空间定义在另外一个命名空间里头。

```
namespace namespace_name1 { 
    export namespace namespace_name2 {
        export class class_name {    } 
    } 
}
```

###### TypeScript 模块

TypeScript 模块的设计理念是可以更换的组织代码。

模块是在其自身的作用域里执行，并不是在全局作用域，这意味着定义在模块里面的变量、函数和类等在模块外部是不可见的，除非明确地使用 export 导出它们。类似地，我们必须通过 import 导入其他模块导出的变量、函数、类等。

两个模块之间的关系是通过在文件级别上使用 import 和 export 建立的。

模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。 大家最熟知的JavaScript模块加载器是服务于 Node.js 的 CommonJS 和服务于 Web 应用的 Require.js。

此外还有有 SystemJs 和 Webpack。

模块导出使用关键字 **export** 关键字，语法格式如下：

```
// 文件名 : SomeInterface.ts 
export interface SomeInterface { 
   // 代码部分
}
```

要在另外一个文件使用该模块就需要使用 **import** 关键字来导入:

import someInterfaceRef = require("./SomeInterface");

###### TypeScript 声明文件

TypeScript 作为 JavaScript 的超集，在开发过程中不可避免要引用其他第三方的 JavaScript 的库。虽然通过直接引用可以调用库的类和方法，但是却无法使用TypeScript 诸如类型检查等特性功能。为了解决这个问题，需要将这些库里的函数和方法体去掉后只保留导出类型声明，而产生了一个描述 JavaScript 库和模块信息的声明文件。通过引用这个声明文件，就可以借用 TypeScript 的各种特性来使用库文件了。

需要使用 declare 关键字来定义它的类型，帮助 TypeScript 判断我们传入的参数类型对不对

```
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```

declare 定义的类型只会用于编译时的检查，编译结果中会被删除。

上例的编译结果是：

```
jQuery('#foo');
```

声明文件

声明文件以 **.d.ts** 为后缀。

声明文件或模块的语法格式如下：

```
declare module Module_Name {
}
```

TypeScript 引入声明文件语法格式：

```
/// <reference path = " runoob.d.ts" />
```
