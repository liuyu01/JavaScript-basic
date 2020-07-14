ES6

##### ECMAScript和JavaScript的关系？

ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现。

ES6的第一个版本，就这样在2015年6月份正式发布一次，作为当年的正式版本。正式名称就是《ECMAScript 2015标准》（简称ES2015）。

ES6既是一个历史名词，也是一个泛指，含义是5.1版以后的JavaScript的下一代标准，涵盖了ES2015、ES2016、ES2017等等，而ES2015则是正式名称，特指该年发布的正式版本的语言标准。

##### Babel转码器

Babel是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。这意味着，你可以用ES6的方式编写程序，又不用担心现有环境是否支持。

##### let 和 const命令

let 声明的变量只在它所在的代码块有效。

for循环的计数器，就很合适使用let 命令。

```
for(let i =0;i<10;i++){
	//...
}
console.log(i);  //ReferenceError: i is not defined
```

###### 不存在变量提升

var命令会发生“变量提升”现象，即变量可以在声明之前使用，值为undefined。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。

```
console.log(foo);//输出undefined
var foo = 2;

console.log(bar); //ReferenceError: bar is not defined
let bar = 2;
```

上面代码中，变量foo用var命令声明，会发生变量提升，即脚本开始运行时，变量foo已经存在了，但是没有值，所以会输出undefined。变量bar用let命令声明，不会发生变量提升。这表示在声明它之前，变量bar是不存在的，这时如果用到它，就会抛出一个错误。

###### 暂时性死区

只要块级作用域内存在let命令，它所声明的变量就“绑定”这个区域，不再受外部的影响。

```
var tmp=123;
if(true){
	tmp='abc';//Uncaught ReferenceError: Cannot access 'tmp' before initialization
	let tmp;
}
```

上面代码中，存在全局变量tmp,但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定这个块级作用域，所以在let声明变量前，对tmp赋值会报错。

ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。

总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称TDZ）

###### 不允许重复声明

###### 为什么需要块级作用域？

ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

第一种场景，内层变量可能会覆盖外层变量。

```
var tmp = new Date();
function f(){
	console.log(tmp);
	if(false){
		var tmp = 'hello world';
	}
}
f();//undefined
```

上面代码的原意是，if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。但是，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。

第二种场景，用来计数的循环变量泄露为全局变量。

```
var s = 'hello';
for(var i=0;i<s.length;i++){
	console.log(s[i]);
}
console.log(i);//5
```

上面代码中，变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。

let实际上为JavaScript新增了块级作用域。

```
function f1(){
	let n = 5;
	if(true){
		let n =10;
	}
	console.log(n);//5
}
f1();
```

上面的函数有两个代码块，都声明了变量n，运行后输出5。这表示外层代码块不受内层代码块的影响。如果两次都是用var定义变量n，最后输出的值才是10。

ES6允许块级作用域的任意嵌套。外层作用域无法读取内层作用域的变量。内层作用域可以定义外层作用域的同名变量。

块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。

```
//IIFE写法
(function(){
	var tmp=...;
}())
//块级作用域写法
{
	let tmp=...;
}
```

立即执行函数的写法

```
(function(a){
	console.log(a); //123,使用()运算符
})(123);
```

```
(function(a){
	console.log(a); //1234，使用()运算符
}(1234));
```

```
!function(a){
	console.log(a); //12345，使用!运算符
}(12345)
```

```
+function(a){
	console.log(a);//123456,使用+运算符
}(123456);
```

```
-function(a){
	console.log(a); //1234567,使用-运算符
}(1234567);
```

```
var fn = function(a){
	console.log(a);  //12345678,使用=运算符
}(12345678);
```

可以看到输出结果，在function前面加! 、+、-甚至是逗号等都可以起到函数定义后立即执行的效果，而()、!、+、-、=等运算符，都将函数声明转换成函数表达式，消除了javascript引擎识别函数表达式和函数声明的歧义，告诉javascript引擎这是一个函数表达式，不是函数声明，可以在后面加括号，并立即执行函数的代码。加括号是最安全的做法，因为！、+、-等运算符还会和函数的返回值进行运算，有时造成不必要的麻烦。

###### 块级作用域与函数声明

ES5规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。

```
//情况一
if(true){
	function f(){}
}
//情况二
try{
	function f(){}
}catch(e){
	//...
}
```

上面两种函数声明，根据ES5的规定都是非法的。

但是，浏览器没有遵守这个规定，为了兼容以前的旧代码，还是支持在块级作用域之中声明函数，因此上面两种情况实际都能运行，不会报错。ES6引入了块级作用域，明确允许在块级作用域之中声明函数。ES6规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。

另外，还有一个需要注意的地方。ES6的块级作用域允许声明函数的规则，只在使用大括号的情况下成立，如果没有使用大扩号，就会报错。

###### const命令

const一旦声明变量，就必须立即初始化，不能留到以后赋值。对于const来说，只声明不赋值，就会报错。

const的作用域与let命令相同：只在声明所在的块级作用域内有效。

const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

###### 本质

const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

###### ES6声明变量的六种方法

ES5只有两种声明变量的方法：var命令和function命令。ES6除了添加let 和const 命令，后面还有两种声明变量的方法：import命令和class命令。所以，ES6一共有6种声明变量的方法。

###### 顶层对象的属性

顶层对象，在浏览器环境指的是window对象，在Node指的是global对象。ES5之中，顶层对象的属性与全局变量是等价的。

```
window.a=1;
a//1
a=2;
window.a //2
```

上面代码中，顶层对象的属性赋值与全局变量的赋值，是同一件事。

顶层对象的属性与全局变量挂钩，被认为是JavaScript语言最大的设计败笔之一。这样的设计带来了几个很大的问题，首先是没法在编译时就报出变量未声明的错误，只有运行时才能知道（因为全局变量可能是顶层对象的属性创造的，而属性的创造是动态的）；其次，程序很容易不知不觉地就创建了全局变量（比如打字出错）；最后，顶层对象的属性是到处 可以读写的，这非常不利于模块编程。另一方面，window对象有实体含义，指的是浏览器的窗口对象，顶层对象是一个有实体含义的对象，也是不合适的。

ES6为了改变这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。也就是说，从ES6开始，全局变量将逐步与顶层对象的属性脱钩。

```
var a=1;
window.a //1

let b=1;
window.b //undefined
```

上面代码中，全局变量a 由var命令声明，所以它是顶层对象的属性；全局变量b由let命令声明，所以它不是顶层对象的属性，返回undefined。

##### 变量的解构赋值

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

ES6允许写成下面这样

let [a,b,c]=[1,2,3];

上面代码表示，可以从数组中提取值，按照对应位置，对变量赋值。

let [x,y, ...z] = ['a'];

x //'a'

y //undefined

z // []

如果解构不成功，变量的值就等于undefined。

###### 默认值

解构赋值允许指定默认值。

let [foo = true] = [];

foo //true

注意，ES6内部使用严格相等运算符（===），判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。

```
let [x=1]=[undefined];
x //1

let[x=1]=[null];
x //null
```

上面代码中，如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined。

###### 对象的解构赋值

解构不仅可以用于数组，还可以用于对象。

```
let {foo, bar} = {foo:'aaa', bar:'bbb'};
foo //'aaa'
bar //'bbb'
```

对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

```
let {baz} = {foo: 'aaa', bar: 'bbb'};
baz //undefined
```

默认值生效的条件是，对象的属性值严格等于undefined。

###### 字符串的解构赋值

字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。

```
const [a,b,c,d,e]='hello';
a //'h'
b //'e'
c //'l'
d //'l'
e //'o'
```

类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。

let {length : len} = 'hello';

len //5

###### 数值和布尔值的解构赋值

解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。

```
let {toString: s} = 123;
s === Number.prototype.toString;  //true

let {toString : s} = true;
s === Boolean.prototype.toString; //true
```

上面代码中，数值和布尔值的包装对象都有toString属性，因此变量s都能取到值。

解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。

```
let {prop: x} = undefined; //Uncaught TypeError
let {prop: y} = null;      //Uncaught TypeError
```

###### 函数参数的解构赋值

函数 的参数也可以使用解构赋值。

```
function add([x, y]){
	return x + y;
}
add([1,2]); //3
```

上面代码中，函数add的参数表面上是一个数组，但在传入参数的那一刻，数组参数就被解构成变量x和y。对于函数内部的代码来说，它们能感受到的参数就是x 和 y。

函数参数的解构也可以使用默认值。

###### 圆括号问题

解构赋值虽然很方便，但是解析起来并不容易。对于编译器来说，一个式子到底是模式，还是表达式，没有办法从一开始就知道，必须解析到（或解析不到）等号才能知道。

由此带来的问题是，如果模式中出现圆括号怎么处理。ES6的规则是，只要有可能导致解构的歧义，就不得使用圆括号。但是，这条规则实际上不那么容易辨别，处理起来相当麻烦。因此，建议只要有可能，就不要在模式中放置圆括号。

变量的解构赋值用途很多：

1）交换变量的值

2）从函数返回多个值  函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，去除这些值就非常方便。

3）函数参数的定义  解构赋值可以方便地将一组参数与变量名对应起来。

4）提取JSON数据  解构赋值对提取JSON对象中的数据，尤其有用。

5）函数参数的默认值

指定参数的默认值，就避免了在函数体内部再写  var foo = config.foo || 'default foo';这样的语句。

6）遍历Map结构

任何部署了Iterator接口的对象，都可以用for...of循环遍历。Map结构原生支持Iterator接口，配合变量的解构赋值，获取键名和键值就非常方便。

7）输入模块的指定方法

加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。

const {SourceMapConsumer, SourceNode} = require('source-map');

##### 字符串 的扩展

###### 字符的Unicode表示法

###### codePointAt()

codePointAt ⽅法是测试⼀个字符由两个字节还是由四个字节组成的最简单⽅法。

###### String.fromCodePoint()

ES5提供String.fromCharCode方法，用于从码点返回对应字符，但是这个方法不能识别32位的UTF-16字符（Unicode编号大于0xFFFF）

ES6提供了String.fromCodePoint方法，可以识别大于0xFFFF的字符，弥补了String.fromCharCode方法的不足。在作用上，正好与codePointAt方法相反。

###### 字符串的遍历器接口

ES6为字符串添加了遍历器接口，使得字符串可以被for...of循环遍历。

除了遍历字符串，这个遍历器最大的优点是可以识别大于0xFFFF的码点，传统的for循环无法识别这样的码点。

###### at()

ES5对字符串对象提供charAt(),返回字符串给定位置的字符。该方法不能识别码点大于oxFFFF的字符。

⽬前，有⼀个提案，提出字符串实例的at ⽅法，可以识别Unicode 编号⼤于0xFFFF 的字符，返回正确的字符。

###### normalize()

ES6提供字符串实例的normalize()方法，用来将字符的不同表示方法统一为同样的形式，这称为Unicode正规化。

```
'\u01D1'.normalize() === '\u004F\u030C'.normalize()
//true
```

###### includes()、startsWith()、endsWith()

传统上，JavaScript只有indexOf()，可以用来确定一个字符串是否包含在另一个字符串中。ES6又提供了三种新方法。

- includes() :返回布尔值，表示是否找到了参数字符串。
- startsWith() :返回布尔值，表示参数字符串是否在原字符串的头部。
- endsWith() :返回布尔值，表示参数字符串是否在原字符串的尾部。

```
let s = 'Hello world!';
s.startsWith('Hello'); //true
s.endsWith('!');//true
s.includes('o');//true
```

这三个方法都支持第二个参数，表示开始搜索的位置。

```
let s = 'Hello world!';
s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```

上面代码表示，使用第二个参数n时，endsWith的行为与其他两个方法有所不同。它针对前n个字符，而其他两个方法针对从第n个位置直到字符串结束。

###### repeat()

repeat方法返回一个新字符串，表示将原字符串重复n次。

```
'x'.repeat(3); //'xxx'
```

参数如果是小数，会被取整。

```
'na'.repeat(2.9); //'nana'
```

如果repeat的参数是负数或者Infinity，会报错。

```
'na'.repeat(Infinity); //Uncaught RangeError: Invalid count value
'na'.repeat(-1); //Uncaught RangeError: Invalid count value
```

但是，如果参数是0到-1之间的小数，则等同于0，这是因为会先进行取整运算。0到-1之间的小数，取整以后等于-0，repeat视同为0。

```
'na'.repeat(-0.9); //""
```

参数NaN等同于0。

```
'na'.repeat(NaN); //''
```

如果repeat的参数是字符串，则会先转换成数字。

```
'na'.repeat('na'); //''
'na'.repeat('3');//'nanana'
```

###### padStart(), padEnd()

ES2017引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。padStart()用于头部补全，padEnd()用于尾部补全。

```
'x'.padStart(5,'ab'); //'ababx'
'x'.padStart(4,'ab'); //'abax'
'x'.padEnd(5,'ab'); //'xabab'
'x'.padEnd(4,'ab'); //'xaba'
```

上面代码中，padStart和padEnd一共接受两个参数，第一个参数用来指定字符串的最小长度，第二个参数是用来补全的字符串。

如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。

```
'xxx'.padStart(2,'ab'); //'xxx'
'xxx'.padEnd(2,'ab'); //'xxx'
```

如果用来补全的字符串与原字符串，两者的长度之和超过了指定的最小长度，则会截去超出位数的补全字符串。

```
'abc'.padStart(10,'0123456789'); //'0123456abc'
```

如果省略第二个参数，默认使用空格补全长度。

```
'x'.padStart(4); //'   x'
'x'.padEnd(4);   //'x   '
```

padStart的常见用途是为数值补全指定位数。下面代码生成10位的数值字符串。

```
'1'.padStart(10,'0') //'0000000001'
'12'.padStart(10,'0') //'0000000012'
'123456'.padStart(10,'0')//'0000123456'
```

另一个用途是提示字符串格式。

```
'12'.padStart(10,'YYYY-MM-DD') //'YYYY-MM-12'
'09-12'.padStart(10,'YYYY-MM-DD') //'YYYY-09-12'
```

##### 模板字符串

模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

```
//普通字符串
`In JavaScript '\n' is a line-feed.`

//多行字符串
`In JavaScript this is
 not legal.`
 
 console.log(`string text line 1
 string text line 2`);
 
 //字符串中嵌入变量
 let name='Blob', time='today';
 `Hello ${name}, how are you ${time}?`
```

上面代码中的模板字符串，都是用反引号表示。如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。

如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。

模板字符串中嵌入变量，需要将变量名写在${}之中。

大括号内部可以放入任意的JavaScript表达式，可以进行运算，以及引用对象属性。

模板字符串之中还能调用函数。

```
function fn(){
	return 'Hello World';
}
`foo ${fn()} bar`  // foo Hello World bar
```

如果大括号中的值不是字符串，将按照一般的规则转为字符串。比如，大括号中是一个对象，将默认调用对象的toString方法。

模板字符串甚至还能嵌套。

##### 标签模板

模板字符串的功能，不仅仅是上面这些。它可以紧跟在一个函数名后面，该函数将被调用来处理这个模板字符串。这被称为‘标签模板’功能（tagged template）

``` 
alert`123`
//等同于
alert(123);
```

标签模板其实不是模板，而是函数调用的一种特殊形式。“标签”指的就是函数，紧跟在后面的模板字符串就是它的参数。

但是，如果模板字符串里面有变量，就不是简单的调用了，而是会将模板字符串先处理成多个参数，再调用函数。

```
let a=5;
let b=10;
tag`Hello ${ a + b } world ${a * b}`;
//等同于
tag(['Hello ',' world ', ''],15,50)

function tag(stringArr, value1, value2){
	//...
}
//等同于
function tag(stringArr,...values){
	//...
}
tag函数的第一个参数是一个数组，该数组的成员是模板字符串中那些没有变量替换的部分。tag函数的其他参数，都是模板字符串各个变量被替换后的值。
```

’标签模板‘的一个重要应用，就是过滤HTML字符串，防止用户输入恶意内容。

标签模板的另一个应用，就是多语言转换（国际化处理）。

##### String.raw()

ES6还为原生的String对象，提供了一个raw方法。

String.raw方法，往往用来充当模板字符串的处理函数，返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串，对应于替换变量后的模板字符串。

```
String.raw`Hi\n${2+3}!`; //"Hi\\n5!"
```

如果原字符串的斜杠已经转义，那么String.raw不会做任何处理。

String.raw方法可以作为处理模板字符串的基本方法，它会将所有变量替换，而且对斜杠进行转义，方便下一步作为字符串来使用。

String.raw方法也可以作为正常的函数使用。这时，它的第一个参数，应该是一个具有raw属性的对象，且raw属性的值应该是一个数组。

##### 数值的扩展

###### 二进制和八进制表示法

ES6提供了二进制和八进制数值的新的写法，分别用前缀0b（或0B）和0o(或0O)表示。

###### Number.isFinite() 、Number.isNaN()

ES6在Number对象上，新提供了Number.isFinite()和Number.isNaN()两个方法。

Number.isFinite()用来检查一个数值是否为有限的（finite）。

Number.isNaN()用来检查一个值是否为NaN。

它们与传统的全局方法 isFinite()和 isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，Number.isFinite()对于非数值一律返回false，Number.isNaN()只有对于NaN才返回true，非NaN一律返回false。

###### Number.parseInt()，Number.parseFloat()

ES6将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。

```
//ES5的写法
parseInt('12.34'); //12
parseFloat('123.45#') //123.45
//ES6的写法
Number.parseInt('12.34') //12
Number.parseFloat('123.45#') //123.45
```

这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

```
Number.parseInt === parseInt //true
Number.parseFloat === parseFloat //true
```

###### Number.isInteger()

Number.isInteger()用来判断一个值是否为整数。需要注意的是，在JavaScript内部，整数和浮点数是同样的储存方法，所以3和3.0被视为同一个值。

```
Number.isInteger(25) //true
Number.isInteger(25.0) //true
Number.isInteger(25.1) //false
Number.isInteger('15') //false
Number.isInteger(true) //false
```

###### Number.EPSILON

ES6在Number对象上面，新增一个极小的常量Number.EPSILON。根据规格，它表示1与大于1的最小浮点数之间的差。

对于64位浮点数来说，大于1的最小浮点数相当于二进制的1.00..001，小数点后面有连续51个零。这个值减去1之后，就等于2的-52次方。

Number.EPSILON实际上是JavaScript能够表示的最小精度。误差如果小于这个值，就可以认为已经没有意义了，即不存在误差了。

Number.EPSILON的实质是一个可以接受的最小误差范围。

###### 安全整数和Number.isSafeInteger()

JavaScript能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表示这个值。ES6引入了Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER这两个常量，用来表示这个范围的上下限。

Number.isSafeInteger()则是用来判断一个整数是否落在这个范围之内。

##### Math对象的扩展

ES6在Math对象上新增了17个与数学相关的方法。所有这些方法都是静态方法，只能在Math对象上调用。

###### Math.trunc()

Math.trunc方法用于去除一个数的小数部分，返回整数部分。

对于非数值，Math.trunc内部使用Number方法将其先转为数值。

对于空值和无法截取整数的值，返回NaN。

###### Math.sign()

Math.sign方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

它会返回5种值：

- 参数为正数，返回+1；
- 参数为负数，返回-1；
- 参数为0，返回0；
- 参数为-0，返回-0；
- 其他值，返回NaN。

如果参数是非数值，会自动转为数值。对于那些无法转为数值的值，会返回NaN。

###### Math.cbrt()

Math.cbrt方法用于计算一个数的立方根。

对于非数值，Math.cbrt方法内部也是先使用Number方法将其转为数值。

###### Math.clz32()

JavaScript的整数使用32位二进制形式表示，Math.clz32方法返回一个数的32位无符号整数形式有多少个前导0。

###### Math.imul()

Math.imul方法返回两个数以32位带符号整数形式相乘的结果，返回的也是一个32位的带符号整数。

如果只考虑最后 32 位，⼤多数情况下，Math.imul(a, b) 与 a * b 的结果是相同的，即该⽅法等同于 

(a * b)|0 的效果（超过32 位的部分溢出）。之所以需要部署这个⽅法，是因为 JavaScript 有精度限制，超过 2 的 53 次⽅的值⽆法精确表示。这就是说，对于那些很⼤的数的乘法，低位数值往往都是不精确的，Math.imul ⽅法可以返回正确的低位数值。

###### Math.fround()

Math.fround方法返回一个数的单精度浮点数形式。

###### Math.hypot()

Math.hypot方法返回所有参数的平方和的平方根。

##### 对数方法

ES6新增了4个对数相关方法。

1）Math.expm1()

Math.expm1(x)返回ex-1,即Math.exp(x)-1。

2）Math.log1p()

Math.log1p(x)方法返回1+x的自然对数，即Math.log(1+x)。如果x小于-1，返回NaN。

3）Math.log10()

Math.log10(x)返回以10为底的x的对数。如果x小于0，则返回NaN。

4)Math.log2()

Math.log2(x)返回以2为底的x的对数。如果x小于0，则返回NaN。

##### 双曲函数方法

ES6新增了6个双曲函数方法。

• Math.sinh(x) 返回 x 的双曲正弦（hyperbolic sine） 

• Math.cosh(x) 返回 x 的双曲余弦（hyperbolic cosine） 

• Math.tanh(x) 返回 x 的双曲正切（hyperbolic tangent） 

• Math.asinh(x) 返回 x 的反双曲正弦（inverse hyperbolic sine） 

• Math.acosh(x) 返回 x 的反双曲余弦（inverse hyperbolic cosine） 

• Math.atanh(x) 返回 x 的反双曲正切（inverse hyperbolic tangent）

##### Math.signbit()

Math.signbit()用来判断一个值的正负，但是如果参数是-0，它会返回-0。

如果参数是NaN，返回false

如果参数是-0，返回true

如果参数是负值，返回true

其他情况返回false

##### 指数运算

ES2016新增了一个指数运算符（**）。

```
2**2  //4
2**3  //8
```

注意，在V8引擎中，指数运算符与Math.pow的实现不相同，对于特别大的运算结果，两者会有细微的差异。

##### Integer数据类型

JavaScript 所有数字都保存成 64 位浮点数，这决定了整数的精确程度只能到 53 个⼆进制位。⼤于这个范围的整数，JavaScript 是⽆法

精确表示的，这使得 JavaScript 不适合进⾏科学和⾦融⽅⾯的精确计算。

现在有⼀个提案，引⼊了新的数据类型 Integer（整数），来解决这个问题。整数类型的数据只⽤来表示整数，没有位数的限制，任何位数的整数都

可以精确表示。

为了与 Number 类型区别，Integer 类型的数据必须使⽤后缀 n 表示。

##### 函数的扩展

##### 函数参数的默认值

ES6之前，不能直接为函数的参数指定默认值，只能采用变通的方法。

```
function log(x,y){
	y = y || 'World';
	console.log(x,y)
}
log('Hello') //Hello World
log('Hello','China') //Hello China
log('Hello','') //Hello World
```

上面代码检查函数log的参数y有没有赋值，如果没有，则指定默认值为World。这种写法的缺点在于，如果参数y赋值了，但是对应的布尔值为false，则该赋值不起作用。就像上面代码的最后一行，参数y等于空字符，结果被改为默认值。

为了二避免这个问题，通常需要先判断一下参数y是否被赋值，如果没有，再等于默认值。

```
if(typeof y === 'undefined'){
	y = 'World';
}
```

ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。

```
function log(x,y='World'){
	console.log(x,y);
}
log('Hello');  //Hello World
log('Hello','China'); //Hello China
log('Hello','') //Hello 
```

可以看到，ES6的写法比ES5简洁许多，而且非常自然。

除了简洁，ES6的写法还有两个好处：首先，阅读代码的人，可以立刻意识到哪些参数是可以省略的，不用查看函数体或文档；其次，有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。

参数变量是默认声明的，所以不能用let或const再次声明。

```
function foo(x=5){
	let x =1; // Uncaught SyntaxError: Identifier 'x' has already been declared
	const x= 2;// Uncaught SyntaxError: Identifier 'x' has already been declared
}
```

使用参数默认值时，函数不能有同名参数。

```
//不报错
function foo(x,x,y){
	//...
}
//报错
function foo(x,x,y=1){
	//...
}
// SyntaxError: Duplicate parameter name not allowed in this context
```

另外，一个容易忽略的地方是，参数默认值不是传值的，而是每次都重新计算默认值表达式的值。也就是说，参数默认值时惰性求值的。

```
let x = 99;
function foo(p = x+1){
	console.log(p);
}
foo(); //100

x=100;
foo();//101 上面代码中，参数p的默认值是x+1。每次调用函数foo，都会重新计算x+1,而不是默认p等于100。
```

###### 与解构赋值默认值结合使用

参数默认值可以与解构赋值的默认值，结合起来使用。

```
function foo({x,y=5}){
	console.log(x,y)
}
foo({}) //undefined 5
foo({x:1}) //1 5
foo({x:1,y:2}) //1 2
foo() //Uncaught TypeError: Cannot destructure property 'x' of 'undefined' as it is undefined.
```

上面代码只使用了对象的解构赋值默认值，没有使用参数参数的默认值。只有当函数foo的参数是一个对象时，变量x和y才会通过解构赋值生成。如果函数foo调用时没提供参数，变量x和y就不会生成，从而报错。通过提供函数参数的默认值，就可以避免这种情况。

```
function foo({x,y=5}=[]){
	console.log(x,y)
}
foo()//undefined 5
```

上面代码指定，如果没有提供参数，函数foo的参数默认为一个空对象。

###### 参数默认值的位置

通常情况下，定义了默认值的参数，应该是函数的尾参数。因为这样比较容易看出来，到底省略了哪些参数。如果非尾部的参数设置默认值，实际上这个参数是没法省略的。

```
function f(x=1,y){
	return [x,y];
}
f() // [1, undefined]
f(2)//[2, undefined]
f(, 1) //Uncaught SyntaxError: Unexpected token ','
f(undefined, 1) // [1, 1]

```

有默认值的参数都不是尾参数。这时，无法只省略该参数，而不是省略它后面的参数，除非显式输入undefined。如果传入undefined，将触发该参数等于默认值，null则没有这个效果。

```
function foo(x=5, y=6){
	console.log(x,y)
}
foo(undefined,null) //5 null
```

上面代码中，x参数对应undefined，结果触发了默认值，y参数等于null，就没有触发默认值。

###### 函数的length属性

指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值以后，length属性将失真。

```
(function (a){}).length //1
(function (a=5){}).length //0
(function (a,b,c=5){}).length //2
```

上面代码中，length属性的返回值，等于函数的参数个数减去指定了默认值的参数个数。这是因为length属性的含义是，该函数预期传入的参数个数。某个参数指定默认值以后，预期传入的参数个数就不包括这个参数了。同理，后文的rest参数也不会计入length属性。

如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了。

```
(function (a=0,b,c){}).length //0
(function (a,b=1,c){}).length //1
```

###### 作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域（context）。等到初始化结束，这个作用域就会消失。这种语法行为，在不设置参数默认值时，是不会出现的。

如果参数的默认值是一个函数，该函数的作用域也遵守这个规则。

##### rest参数

ES6引入rest参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了。rest参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

arguments 对象不是数组，⽽是⼀个类似数组的对象。所以为了使⽤数组的⽅法，必须使⽤ 

Array.prototype.slice.call 先将其转为数组。

注意，rest参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。

```
function f(a,...b,c){
	//...
}
Uncaught SyntaxError: Rest parameter must be last formal parameter
```

函数的length属性，不包括rest参数。

##### 严格模式

从ES5开始，函数内部可以设定为严格模式。

```
function doSomething(a,b){
	'use strict';
	//code
}
```

ES2016做了一点修改，规定只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显示设定为严格模式，否则会报错。

这样规定的原因是，函数内部的严格模式，同时适用于函数体和函数参数。但是，函数执行的时候，先执行函数参数，然后再执行函数体。这样就有一个不合理的地方，只有从函数体之中，才能知道参数是否应该以严格模式执行，但是参数却应该先于函数体执行。

##### name属性

函数的name属性，返回该函数的函数名。

```
function foo(){}
foo.name  //'foo'
```

这个属性早就被浏览器广泛支持，但是直到ES6，才将其写入了标准。

需要注意的是，ES6对这个属性的行为做出了一些修改。如果将一个匿名函数赋值给一个变量，ES5的name属性，会返回空字符串，而ES6的name属性会返回实际的函数名。

```
var f = function (){};
//ES5
f.name  // ''

//ES6
f.name // 'f'
```

如果将一个具名函数赋值给一个变量，则ES5和ES6的name属性都返回这个具名函数原本的名字。

```
const bar = function baz(){};
//ES5
bar.name  // 'baz'
//ES6
bar.name //'baz'
```

Function 构造函数返回的函数实例，name 属性的值为 anonymous。 

(**new** Function).name *// "anonymous"*

bind 返回的函数，name 属性值会加上 bound 前缀。

**function** foo() {};

foo.bind({}).name *// "bound foo"*

(**function**(){}).bind({}).name *// "bound "*

##### 箭头函数

ES6允许使用“箭头”（=>）定义函数。

```
var f =v =>v;
//等同于
var f = function(v){
	return v;
}
```

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。

```
var f = ()=> 5;
//等同于
var f = function(){return 5;}
```

如果箭头函数的代码块部分多余一条语句，就要使用大括号将它们括起来，并且使用return语句返回。

```
var sum = (num1,num2)=>{return num1+num2;}
```

由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。

```
//报错
let getTempItem = id => {id: id, name: 'Temp'};
//不报错
let getTempItem = id => ({id: id, name: 'Temp'});
```

如果箭头函数只有一行语句，且不需要返回值，可以采用下面的写法，就不用大括号了。

```
let fn = () => void doesNotReturn();
```

箭头函数有几个使用注意点：

1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用rest参数代替。

4）不可以使用yield命令，因此箭头函数不能用作Generator函数。

上面四点中，第一点尤其值得注意。this对象的指向是可变的，但是在箭头函数中，它是固定的。

this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为它没有this,所以呀就不能用作构造函数。

由于箭头函数没有自己的this，所以当然也就不能用call()、appl()、bind()这些方法去改变this的指向。

##### 双冒号运算符

箭头函数可以绑定this对象，大大减少了显式绑定this对象的写法（）（call、apply、bind）。但是，箭头函数并不适⽤于所有场合，所以现在有⼀个提案，提出了 “函数绑定”（function bind）运算符，⽤来取代 call、apply、bind 调⽤。

函数绑定运算符是并排的两个冒号（::），双冒号左边是⼀个对象，右边是⼀个函数。该运算符会⾃动将左边的对象，作为上下⽂环境（即 this对象），绑定到右边的函数上⾯。

```
foo::bar;
//等同于
bar.bind(foo);
```

如果双冒号左边为空，右边是⼀个对象的⽅法，则等于将该⽅法绑定在该对象上⾯。

双冒号运算符的运算结果，还是⼀个对象，因此可以采⽤链式写法。

##### 尾调用优化

###### 什么 尾调用？

尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数。

```
function f(x){
	return g(x);
}
```

上面代码中，函数f的最后一步是调用函数g，这就叫尾调用。

以下三种情况，都不属于尾调用。

```
//情况一
function f(x){
	let y = g(x);
	return y;
}
//情况二
function f(x){
	return g(x)+1;
}
//情况三
function f(x){
	g(x);
}
```

上面代码中，情况一是调用函数g之后，还有赋值操作，所以不属于尾调用。情况二也属于调用后还有操作，即使写在一行内。情况三等同于下面的代码。

```
function f(x){
	g(x);
	return undefined;
}
```

尾调用不一定出现在函数尾部，只要是最后一步操作即可。

###### 尾调用优化

尾调用之所以与其他调用不同，就在于它的特殊的调用位置。

我们知道，函数调用会在内存形成一个“调用记录”，又称“调用帧”（call frame），保存调用位置和内部变量等信息。如果在函数A的内部调用函数B，那么在A的调用帧上方，还会形成一个B的调用帧。等到B运行结束，将结果返回到A，B的调用帧才会消失。如果函数B内部还调用函数C，那就还有一个C的调用帧，以此类推。所有的调用帧，就形成一个“调用栈”（call stack）。

尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧，因为调用位置、内部变量等信息都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。

```
function f(){
	let m =1;
	let n =2;
	return g(m+n);
}
f();

//等同于
function f(){
	return g(3);
}
f();
//等同于
g(3);
```

上⾯代码中，如果函数 g 不是尾调⽤，函数 f 就需要保存内部变量 m 和 n 的值、g 的调⽤位置等信息。但由于调⽤ g 之后，函数 f 就结束了，所以执⾏到最后⼀步，完全可以删除 f(x) 的调⽤帧，只保留 g(3) 的调⽤帧。这就叫做 “尾调⽤优化”（Tail call optimization），即只保留内层函数的调⽤帧。如果所有函数都是尾调⽤，那么完全可以做到每次执⾏时，调⽤帧只有⼀项，这将⼤⼤节省内存。这就是 “尾调⽤优化” 的意义。

注意，只有不再⽤到外层函数的内部变量，内层函数的调⽤帧才会取代外层函数的调⽤帧，否则就⽆法进⾏ “尾调⽤优化”。

##### 尾递归

函数调用自身，称为递归。如果尾调用自身，就称为尾递归。

递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow)。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。

```
function factorial(n){
	if(n===1) return 1;
	return n * factorial(n-1);
}
factorial(5);//120
```

上面代码是一个阶乘函数，计算n的阶乘，最多需要保存n个调用记录，复杂度0(n)。

如果改写成尾递归，就只保留一个调用记录，复杂度0(1)。

```
function factorial(n,total){
	if(n===1) return total;
	return factorial(n-1, n*total);
}
factorial(5,1);
```

由此可⻅，“尾调⽤优化” 对递归操作意义重⼤，所以⼀些函数式编程语⾔将其写⼊了语⾔规格。ES6 是如此，第⼀次明确规定，所有 ECMAScript的实现，都必须部署 “尾调⽤优化”。这就是说，ES6 中只要使⽤尾递归，就不会发⽣栈溢出，相对节省内存。

###### 递归函数的改写

尾递归的实现，往往需要改写递归函数，确保最后一步只调用自身。做到这一点的方法，就是把所有用到的内部变量改写成函数的参数。

两个方法可以解决这个问题。方法一是在尾递归函数之外，再提供一个正常形式的函数。

```
function tailFactorial(n, total) {
	if (n === 1) return total;
	return tailFactorial(n - 1, n * total); 
}
function factorial(n) {
	return tailFactorial(n, 1); 
}
factorial(5) // 120
```

函数式编程有⼀个概念，叫做柯⾥化（currying），意思是将多参数的函数转换成单参数的形式。这⾥也可以使⽤柯⾥化。

```
function currying(fn, n) {
	return function (m) {
		return fn.call(this, m, n);
	};
}
function tailFactorial(n, total) {
	if (n === 1) return total;
	return tailFactorial(n - 1, n * total); 
}
const factorial = currying(tailFactorial, 1);
factorial(5) // 120
上⾯代码通过柯⾥化，将尾递归函数 tailFactorial 变为只接受⼀个参数的 factorial。

```

第⼆种⽅法就简单多了，就是采⽤ ES6 的函数默认值。

```
function factorial(n, total = 1) {
	if (n === 1) return total;
	return factorial(n - 1, n * total); 
}
factorial(5) // 120
```

上⾯代码中，参数 total 有默认值 1，所以调⽤时不⽤提供这个值。

总结⼀下，递归本质上是⼀种循环操作。纯粹的函数式编程语⾔没有循环操作命令，所有的循环都⽤递归实现，这就是为什么尾递归对这些语⾔极其重要。对于其他⽀持 “尾调⽤优化” 的语⾔（⽐如 Lua，ES6），只需要知道循环可以⽤递归代替，⽽⼀旦使⽤递归，就最好使⽤尾递归。

###### 严格模式

ES6 的尾调⽤优化只在严格模式下开启，正常模式是⽆效的。

这是因为在正常模式下，函数内部有两个变量，可以跟踪函数的调⽤栈。

• func.arguments：返回调⽤时函数的参数。

• func.caller：返回调⽤当前函数的那个函数。

尾调⽤优化发⽣时，函数的调⽤栈会改写，因此上⾯两个变量就会失真。严格模式禁⽤这两个变量，所以尾调⽤模式仅在严格模式下⽣效。

```
function restricted() {
	'use strict';
	restricted.caller; // 报错
	restricted.arguments; // 报错
}
restricted();
```

###### 尾递归优化的实现

尾递归优化只在严格模式下⽣效，那么正常模式下，或者那些不⽀持该功能的环境中，有没有办法也使⽤尾递归优化呢？回答是可以的，就是⾃⼰实现尾递归优化。

它的原理⾮常简单。尾递归之所以需要优化，原因是调⽤栈太多，造成溢出，那么只要减少调⽤栈，就不会溢出。怎么做可以减少调⽤栈呢？就是采⽤“循环” 换掉 “递归”。保证了调⽤栈只有⼀层。

蹦床函数（trampoline）可以将递归执⾏转为循环执⾏。

##### 函数参数的尾逗号

ES2017 允许函数的最后⼀个参数有尾逗号（trailing comma）。

此前，函数定义和调⽤时，都不允许最后⼀个参数后⾯出现逗号。

如果像上⾯这样，将参数写成多⾏（即每个参数占据⼀⾏），以后修改代码的时候，想为函数 

clownsEverywhere 添加第三个参数，或者调整参数的次序，就势必要在原来最后⼀个参数后⾯添加⼀个逗号。这对于版本管理系统来说，就会显示添加逗号的那⼀⾏也发⽣了变动。这看上去有点冗余，因此新的语法允许定义和调⽤时，尾部直接有⼀个逗号。

这样的规定也使得，函数参数与数组和对象的尾逗号规则，保持⼀致了。

##### catch语句的参数

⽬前，有⼀个提案，允许 try...catch 结构中的 catch 语句调⽤时不带有参数。

传统的写法是 catch 语句必须带有参数，⽤来接收 try 代码块抛出的错误。

```
try{
	//...
}catch(error){
	//...
}
```

新的写法允许省略catch后面的参数，而不报错。

```
try {
// ···
} catch {
// ···
}
```

新写法只在不需要错误实例的情况下有⽤，因此不及传统写法的⽤途⼴。

##### 数组的扩展

###### 扩展运算符

扩展运算符（spread）是三个点（...）。它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```
console.log(...[1, 2, 3])
// 1 2 3
```

###### 替代数组的apply方法

由于扩展运算符可以展开数组，所以不再需要apply ⽅法，将数组转为函数的参数了。

```
//ES5的写法
function f(x, y, z){
	//...
}
var args = [0, 1, 2];
f.apply(null, args);

//ES6的写法
function f(x, y, z){
	//...
}
let args = [0, 1, 2];
f(...args);
```

###### 扩展运算符的应用

（1）复制数组

数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。

```
//ES5只能用变通方法来复制数组。
const a1 = [1, 2];
const a2 = a1.concat();
//ES6写法
const a1 = [1, 2];
const a2 = [...a1];
```

(2)合并数组

扩展运算符提供了数组合并的新写法。

```
//ES5
[1, 2].concat(more)
var arr1 = ['a','b'];
var arr2 = ['c'];
var arr3 = ['d','e'];
arr1.concat(arr2,arr3);
//es6
[1,2,...more]
[...arr1,...arr2,...arr3]
```

(3)与解构赋值结合

扩展运算符可以与解构赋值结合起来，用于生成数组。

```
//es5
a=list[0],rest=list.slice(1)
//es6
[a,...rest]=list;
```

(4)字符串

扩展运算符还可以将字符串转为真正的数组。

```
[...'hello']
// ["h", "e", "l", "l", "o"]
```

(5)实现了Iterator接口的对象

任何Iterator 接⼝的对象（参阅Iterator ⼀章），都可以⽤扩展运算符转为真正的数组。

(6)Map 和 Set结构，Generator函数

扩展运算符内部调⽤的是数据结构的Iterator 接⼝，因此只要具有Iterator 接⼝的对象，都可以使⽤扩展运算符，⽐如Map 结构。

Generator 函数运⾏后，返回⼀个遍历器对象，因此也可以使⽤扩展运算符。对这个遍历器对象执⾏扩展运算符，就会将内部遍历得到的值，转为⼀个数组。
如果对没有Iterator 接⼝的对象，使⽤扩展运算符，将会报错。

##### Array.from()

Array.from方法用于将两类对象转为真正的数组：类似数组的对象和可遍历的对象（包括ES6新增的数据结构SetH和Map）。

```
let arrayLike = {
	'0':'a',
	'1':'b',
	'2':'c',
	length:3
};

//ES5的写法
 var arr1 = [].slice.call(arrayLike); //["a", "b", "c"]
 
//ES6的写法
let arr2 = Array.from(arrayLike); // ["a", "b", "c"]
 
```

值得提醒的是，扩展运算符（...）也可以将某些数据结构转为数组。

扩展运算符背后调⽤的是遍历器接⼝（Symbol.iterator），如果⼀个对象没有部署这个接⼝，就⽆法转换。Array.from ⽅法还⽀持
类似数组的对象。所谓类似数组的对象，本质特征只有⼀点，即必须有length 属性。因此，任何有length 属性的对象，都可以通过Array.from ⽅法转为数组，⽽此时扩展运算符就⽆法转换。

对于还没有部署该⽅法的浏览器，可以⽤Array.prototype.slice ⽅法替代。

Array.from 还可以接受第⼆个参数，作⽤类似于数组的map ⽅法，⽤来对每个元素进⾏处理，将处理后的值放⼊返回的数组。

##### Array.of()

Array.of 方法用于将一组值，转换为数组。

```
Array.of(3,11,8) //[3, 11, 8]
Array.of(3) // [3]
Array.of(3).length // 1
```

这个⽅法的主要⽬的，是弥补数组构造函数Array() 的不⾜。因为参数个数的不同，会导致Array() 的⾏为有差异。

```
Array() // []
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]
```

上⾯代码中，Array ⽅法没有参数、⼀个参数、三个参数时，返回结果都不⼀样。只有当参数个数不少于2 个时，Array() 才会返回由参数组成的新数组。参数个数只有⼀个时，实际上是指定数组的⻓度。

Array.of 基本上可以⽤来替代Array() 或new Array()，并且不存在由于参数不同⽽导致的重载。它的⾏为⾮常统⼀。

```
Array.of() // []
Array.of(undefined) // [undefined]
Array.of(1) // [1]
Array.of(1, 2) // [1, 2]
```

Array.of 总是返回参数值组成的数组。如果没有参数，就返回⼀个空数组。

Array.of ⽅法可以⽤下⾯的代码模拟实现。

```
function ArrayOf(){
	return [].slice.call(arguments);
}
```

##### 数组实例的copyWithin()

数组实例的copyWithin ⽅法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使⽤这个⽅法，会修改当前数组。
Array.prototype.copyWithin(target, start = 0, end = this.length)
它接受三个参数。
• target（必需）：从该位置开始替换数据。
• start（可选）：从该位置开始读取数据，默认为0。如果为负值，表示倒数。
• end（可选）：到该位置前停⽌读取数据，默认等于数组⻓度。如果为负值，表示倒数。

##### 数组实例的find()和findIndex()

数组实例的find ⽅法，⽤于找出第⼀个符合条件的数组成员。它的参数是⼀个回调函数，所有数组成员依次执⾏该回调函数，直到找出第⼀个返回值为true 的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。

```
[1, 5, 10, 15].find(function(value, index, arr) {
return value > 9;
}) // 10
```

上⾯代码中，find ⽅法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。

数组实例的findIndex ⽅法的⽤法与find ⽅法⾮常类似，返回第⼀个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

```
[1, 5, 10, 15].findIndex(function(value, index, arr) {
return value > 9;
}) // 2
```

这两个⽅法都可以接受第⼆个参数，⽤来绑定回调函数的this 对象。
另外，这两个⽅法都可以发现NaN，弥补了数组的IndexOf ⽅法的不⾜。

##### 数组实例的fill()

fill ⽅法使⽤给定值，填充⼀个数组。

```
['a', 'b', 'c'].fill(7)
// [7, 7, 7]
new Array(3).fill(7)
// [7, 7, 7]

```

上⾯代码表明，fill ⽅法⽤于空数组的初始化⾮常⽅便。数组中已有的元素，会被全部抹去。
fill ⽅法还可以接受第⼆个和第三个参数，⽤于指定填充的起始位置和结束位置。

```
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```

##### 数组实例的entries()，keys()和values()

ES6 提供三个新的⽅法——entries()，keys() 和values()——⽤于遍历数组。它们都返回⼀个遍历器对象（详⻅《Iterator》⼀章）,可以⽤for...of 循环进⾏遍历，唯⼀的区别是keys() 是对键名的遍历、values() 是对键值的遍历，entries() 是对键值对的遍历。

##### 数组实例的includes()

Array.prototype.includes ⽅法返回⼀个布尔值，表示某个数组是否包含给定的值，与字符串的includes ⽅法类似。ES2016引⼊了该⽅法。

```
[1, 2, 3].includes(2) // true
[1, 2, 3].includes(4) // false
[1, 2, NaN].includes(NaN) // true
```

该⽅法的第⼆个参数表示搜索的起始位置，默认为0。如果第⼆个参数为负数，则表示倒数的位置，如果这时它⼤于数组⻓度（⽐如第⼆个参数为-4，但数组⻓度为3），则会重置为从0 开始。

没有该⽅法之前，我们通常使⽤数组的indexOf ⽅法，检查是否包含某个值。

indexOf ⽅法有两个缺点，⼀是不够语义化，它的含义是找到参数值的第⼀个出现位置，所以要去⽐较是否不等于-1，表达起来不够直观。⼆是，它内部使⽤严格相等运算符（===）进⾏判断，这会导致对NaN 的误判。

```
[NaN].indexOf(NaN)
// -1
```

includes 使⽤的是不⼀样的判断算法，就没有这个问题。

```
[NaN].includes(NaN)
// true
```

另外，Map 和Set 数据结构有⼀个has ⽅法，需要注意与includes 区分。

• Map 结构的has ⽅法，是⽤来查找键名的，⽐如Map.prototype.has(key)、WeakMap.prototype.has(key)、
Reflect.has(target, propertyKey)。
• Set 结构的has ⽅法，是⽤来查找值的，⽐如Set.prototype.has(value)、WeakSet.prototype.has(value)。

##### 数组的空位

数组的空位值，数组的某一个位置没有任何值。比如，Array构造函数返回的数组都是空位。

``` 
Array(3)// [, , ,]
```

上⾯代码中，Array(3) 返回⼀个具有3 个空位的数组。
注意，空位不是undefined，⼀个位置的值等于undefined，依然是有值的。空位是没有任何值，in 运算符可以说明这⼀点。

```
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```

上⾯代码说明，第⼀个数组的0 号位置是有值的，第⼆个数组的0 号位置没有值。

ES5 对空位的处理，已经很不⼀致了，⼤多数情况下会忽略空位。
• forEach(), filter(), every() 和some() 都会跳过空位。
• map() 会跳过空位，但会保留这个值
• join() 和toString() 会将空位视为undefined，⽽undefined 和null 会被处理成空字符串。

ES6 则是明确将空位转为undefined。
Array.from ⽅法会将数组的空位，转为undefined，也就是说，这个⽅法不会忽略空位。

扩展运算符（...）也会将空位转为undefined。

copyWithin() 会连空位⼀起拷⻉。

fill() 会将空位视为正常的数组位置。

for...of 循环也会遍历空位。

entries()、keys()、values()、find() 和findIndex() 会将空位处理成undefined。

##### 对象的扩展

###### 属性的简洁表示法

ES6允许直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。

```
const foo= 'bar';
const baz= {foo};
baz //{foo:'bar'}
//等同于
const baz = {foo:foo}
```

上面代码表明，ES6允许在对象之中，直接写变量。这时，属性名为变量名，属性值为变量的值。

```
function f(x,y){
	return {x,y};
}
//等同于
function f(x,y){
	return {x:x,y:y};
}
f(1,2) //{x: 1, y: 2}
```

除了属性简写，方法也可以简写。

```
const o = {
	method(){
		return 'Hello!';
	}
};
//等同于
const o = {
	method: function(){
		return 'Hello!';
	}
};
```

###### 属性名表达式

JavaScript定义对象的属性，有两种方法。

```
//方法一
obj.foo = true;
//方法二
obj['a'+'bc'] = 123;
```

方法一是直接用标识符作为属性名，方法二是用表达式作为属性名，这时要将表达式放在方括号之内。

是，如果使⽤字⾯量⽅式定义对象（使⽤⼤括号），在ES5 中只能使⽤⽅法⼀（标识符）定义属性。

```
var obj = {
  foo: true,
  abc: 123
};
```

ES6 允许字⾯量定义对象时，⽤⽅法⼆（表达式）作为对象的属性名，即把表达式放在⽅括号内。

```
let propKey = 'foo';
let obj = {
	[propKey]: true,
	['a'+'bc']: 123
};
```

表达式还可以用于定义方法名。

```
let obj = {
	['h' + 'ello'](){
		return 'hi';
	}
};
obj.hello(); //hi
```

注意，属性名表达式与简洁表示法，不能同时使用，会报错

```
// 报错
const foo = 'bar';
const bar = 'abc';
const baz = { [foo] };
// 正确
const foo = 'bar';
const baz = { [foo]: 'abc'};
```

###### 方法的name属性

函数的 name属性，返回函数名。对象方法也是函数，因此也有name属性。

```
const person = {
	sayName() {
		console.log('hello!');
	},
};
person.sayName.name // "sayName"
```

⾯代码中，⽅法的name 属性返回函数名（即⽅法名）。

如果对象的⽅法使⽤了取值函数（getter）和存值函数（setter），则name 属性不是在该⽅法上⾯，⽽是该⽅法的属性的描述对象的get和set 属性上⾯，返回值是⽅法名前加上get 和set。

```
const obj = {
	get foo() {},
	set foo(x) {}
};
obj.foo.name // TypeError: Cannot read property 'name' of undefined
const descriptor = Object.getOwnPropertyDescriptor(obj, 'foo');
descriptor.get.name // "get foo"
descriptor.set.name // "set foo"
```

有两种特殊情况：bind ⽅法创造的函数，name 属性返回bound 加上原函数的名字；Function 构造函数创造的函数，name 属性返回anonymous。

```
(new Function()).name // "anonymous"
var doSomething = function() {
// ...
};
doSomething.bind().name // "bound doSomething"
```

如果对象的⽅法是⼀个Symbol 值，那么name 属性返回的是这个Symbol 值的描述。

```
const key1 = Symbol('description');
const key2 = Symbol();
let obj = {
[key1]() {},
[key2]() {},
};
obj[key1].name // "[description]"
obj[key2].name // ""
上⾯代码中，key1 对应的Symbol 值有描述，key2 没有。
```

###### Object.is()

ES5比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。JavaScript 缺乏⼀种运算，在所有环境中，只要两个值是⼀样的，它们就应该相等。

ES6 提出“Same-value equality”（同值相等）算法，⽤来解决这个问题。Object.is 就是部署这个算法的新⽅法。它⽤来⽐较两个值是否严格相等，与严格⽐较运算符（===）的⾏为基本⼀致。

```
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false
不同之处只有两个：⼀是+0 不等于-0，⼆是NaN 等于⾃身。
+0 === -0 //true
NaN === NaN // false
Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

###### Object.assign()

Object.assign方法用于对象的合并，将源对象(source)的所有可枚举属性，复制到目标对象(target)。

```
const target= {a: 1};
const source1= {b: 2};
const source2= {c: 3};
Object.assign(target,source1,source2);
target //{a: 1, b: 2, c: 3}
```

Object.assign ⽅法的第⼀个参数是⽬标对象，后⾯的参数都是源对象。
注意，如果⽬标对象与源对象有同名属性，或多个源对象有同名属性，则后⾯的属性会覆盖前⾯的属性。

```
const target = { a: 1, b: 1 };
const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

如果只有⼀个参数，Object.assign 会直接返回该参数。

```
const obj = {a: 1};
Object.assign(obj) === obj // true
```

如果该参数不是对象，则会先转成对象，然后返回。

```
typeof Object.assign(2) // "object"
```

由于undefined 和null ⽆法转成对象，所以如果它们作为参数，就会报错。

```
Object.assign(undefined) // 报错
Object.assign(null) // 报错
```

如果⾮对象参数出现在源对象的位置（即⾮⾸参数），那么处理规则有所不同。⾸先，这些参数都会转成对象，如果⽆法转成对象，就会跳过。这意味着，如果undefined 和null 不在⾸参数，就不会报错。

```
let obj = {a: 1};
Object.assign(obj, undefined) === obj // true
Object.assign(obj, null) === obj // true
```

其他类型的值（即数值、字符串和布尔值）不在⾸参数，也不会报错。但是，除了字符串会以数组形式，拷⻉⼊⽬标对象，其他值都不会产⽣效果。

Object.assign 拷⻉的属性是有限制的，只拷⻉源对象的⾃身属性（不拷⻉继承属性），也不拷⻉不可枚举的属性（enumerable: false）。

注意点：

(1)浅拷贝 Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

```
const obj1= {a: {b: 1}};
const obj2= Object.assign({},obj1);
obj1.a.b=2;
obj2.a.b //2
```

上⾯代码中，源对象obj1 的a 属性的值是⼀个对象，Object.assign 拷⻉得到的是这个对象的引⽤。这个对象的任何变化，都会反映到⽬标对象上⾯。

(2)同名属性的替换

对于这种嵌套的对象，一旦遇到同名属性，Object.assign的处理方法是替换，而不是添加。

```
const target = {a: {b: 'c', d: 'e'}};
const source = {a: {b: 'hello'}};
Object.assign(target,source);// { a: { b: 'hello' } }
```

(3)数组的处理

Object.assign可以用来处理数组，但是会把数组视为对象。

```
Object.assign([1, 2, 3], [4, 5]) //[4, 5, 3]
```

上⾯代码中，Object.assign 把数组视为属性名为0、1、2 的对象，因此源数组的0 号属性4 覆盖了⽬标数组的0 号属性1。

(4)取值函数的处理

Object.assign只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。

```
const source = {
	get foo(){ return 1}
}
const target = {};
Object.assign(target, source); //{foo: 1}
```

上⾯代码中，source 对象的foo 属性是⼀个取值函数，Object.assign 不会复制这个取值函数，只会拿到值以后，将这个值复制过去。

###### 常见用途

(1)为对象添加属性

(2)为对象添加方法

(3)克隆对象

(4)合并多个对象

(5)为属性指定默认值

###### 属性的可枚举性和遍历

可枚举性：对象的每个属性都有一个描述对象（Descriptor）,用来控制该属性的行为。Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象。

```
let obj={foo : 123};
Object.getOwnPropertyDescriptor(obj, 'foo');
//{value: 123, writable: true, enumerable: true, configurable: true}
```

描述对象的enumerable属性，称为‘可枚举性’，如果该属性为false,就表示某些操作会忽略当前属性。

目前，有四个操作会忽略enumerable为false的属性。

- for...in循环：只遍历对象自身的和继承的可枚举的属性。
- Object.keys():返回对象自身的所有可枚举的属性的键名。
- JSON.stringify():只串行化对象自身的可枚举的属性。
- Object.assign():忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。

这四个操作之中，前三个是ES5 就有的，最后⼀个Object.assign() 是ES6 新增的。其中，只有for...in 会返回继承的属性，其他三个⽅法都会忽略继承的属性，只处理对象⾃身的属性。实际上，引⼊“可枚举”（enumerable）这个概念的最初⽬的，就是让某些属性可以规避掉for...in 操作，不然所有内部属性和⽅法都会被遍历到。⽐如，对象原型的toString ⽅法，以及数组的length 属性，就通过“可枚举性”，从⽽避免被for...in 遍历到。

另外，ES6 规定，所有Class 的原型的⽅法都是不可枚举的。

总的来说，操作中引⼊继承的属性会让问题复杂化，⼤多数时候，我们只关⼼对象⾃身的属性。所以，尽量不要⽤for...in 循环，⽽⽤Object.keys() 代替。

属性的遍历

ES6一共有5种方法可以遍历对象的属性。

(1)for...in

for...in 循环遍历对象自身的和继承的可枚举属性（不含Symbol属性）

(2)Object.keys(obj)

Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含Symbol属性）的键名。

(3)Object.getOwnPropertyNames(obj)

Object.getOwnPropertyNames 返回一个数组，包含对象自身的所有属性（不含Symbol属性，但是包括不可枚举属性）的键名。

(4)Object.getOwnPropertySymbols(obj)

Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有Symbol属性的键名。

(5)Reflect.ownKeys(obj)

Reflect.ownKeys返回一个数组，包含对象自身的所有键名，不管键名是Symbol或字符串，也不管是否可枚举。

以上的5 种⽅法遍历对象的键名，都遵守同样的属性遍历的次序规则。
• ⾸先遍历所有数值键，按照数值升序排列。
• 其次遍历所有字符串键，按照加⼊时间升序排列。
• 最后遍历所有Symbol 键，按照加⼊时间升序排列。

###### Object.getOwnPropertyDescriptors()

前⾯说过，Object.getOwnPropertyDescriptor ⽅法会返回某个对象属性的描述对象（descriptor）。ES2017 引⼊了Object.getOwnPropertyDescriptors ⽅法，返回指定对象所有⾃身属性（⾮继承属性）的描述对象。

Object.getOwnPropertyDescriptors ⽅法返回⼀个对象，所有原对象的属性名都是该对象的属性名，对应的属性值就是该属性的描述对象。

该⽅法的引⼊⽬的，主要是为了解决Object.assign() ⽆法正确拷⻉get 属性和set 属性的问题。

######   

```
__proto__ 属性，Object.setPrototypeOf()，Object.getPrototypeOf()
```

JavaScript 语⾔的对象继承是通过原型链实现的。ES6 提供了更多原型对象的操作⽅法。

__ proto__ 属性（前后各两个下划线），⽤来读取或设置当前对象的prototype 对象。⽬前，所有浏览器（包括IE11）都部署了这个属性。

```
实现上，__proto__ 调⽤的是Object.prototype.__proto__，
```

Object.setPrototypeOf()

Object.setPrototypeOf ⽅法的作⽤与__ proto__ 相同，⽤来设置⼀个对象的prototype 对象，返回参数对象本身。它是ES6 正式推荐的设置原型对象的⽅法。

如果第⼀个参数不是对象，会⾃动转为对象。但是由于返回的还是第⼀个参数，所以这个操作不会产⽣任何效果。

由于undefined 和null ⽆法转为对象，所以如果第⼀个参数是undefined 或null，就会报错。

Object.getPrototypeOf()

该⽅法与Object.setPrototypeOf ⽅法配套，⽤于读取⼀个对象的原型对象。

如果参数不是对象，会被⾃动转为对象。

如果参数是undefined 或null，它们⽆法转为对象，所以会报错。

###### super关键字

我们知道，this 关键字总是指向函数所在的当前对象，ES6 ⼜新增了另⼀个类似的关键字super，指向当前对象的原型对象。

```
const proto = {
	foo: 'hello'
};
const obj = {
	find() {
		return super.foo;
	}
};
Object.setPrototypeOf(obj, proto);
obj.find() // "hello"
```

上⾯代码中，对象obj 的find ⽅法之中，通过super.foo 引⽤了原型对象proto 的foo 属性。
注意，super 关键字表示原型对象时，只能⽤在对象的⽅法之中，⽤在其他地⽅都会报错。

###### Object.keys()，Object.values()，Object.entries()

Object.keys() ：ES5 引⼊了Object.keys ⽅法，返回⼀个数组，成员是参数对象⾃身的（不含继承的）所有可遍历（enumerable）属性的键名。

ES2017 引⼊了跟Object.keys 配套的Object.values 和Object.entries，作为遍历⼀个对象的补充⼿段，供for...of
循环使⽤。

Object.values()：Object.values ⽅法返回⼀个数组，成员是参数对象⾃身的（不含继承的）所有可遍历（enumerable）属性的键值。

Object.values 只返回对象⾃身的可遍历属性。

Object.values 会过滤属性名为Symbol 值的属性。

如果Object.values ⽅法的参数是⼀个字符串，会返回各个字符组成的⼀个数组。

如果参数不是对象，Object.values 会先将其转为对象。由于数值和布尔值的包装对象， 都不会为实例添加⾮继承的属性。所以，Object.values 会返回空数组。

Object.entries()：Object.entries ⽅法返回⼀个数组，成员是参数对象⾃身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。

除了返回值不⼀样，该⽅法的⾏为与Object.values 基本⼀致。
如果原对象的属性名是⼀个Symbol 值，该属性会被忽略。

Object.entries 的基本⽤途是遍历对象的属性。

Object.entries ⽅法的另⼀个⽤处是，将对象转为真正的Map 结构。

###### 对象的扩展运算符(...)

（1）解构赋值

对象的解构赋值⽤于从⼀个对象取值，相当于将所有可遍历的、但尚未被读取的属性，分配到指定的对象上⾯。所有的键和它们的值，都会拷⻉到新对象上⾯。

由于解构赋值要求等号右边是⼀个对象，所以如果等号右边是undefined 或null，就会报错，因为它们⽆法转为对象。

解构赋值必须是最后⼀个参数，否则会报错。

```
let { ...x, y, z } = obj; // 句法错误
let { x, ...y, ...z } = obj; // 句法错误
```

注意，解构赋值的拷⻉是浅拷⻉，即如果⼀个键的值是复合类型的值（数组、对象、函数）、那么解构赋值拷⻉的是这个值的引⽤，⽽不是这个值的副本。

另外，扩展运算符的解构赋值，不能复制继承⾃原型对象的属性。

```
let o1 = { a: 1 };
let o2 = { b: 2 };
o2.__proto__ = o1;
let { ...o3 } = o2;
o3 // { b: 2 }
o3.a // undefined
```

解构赋值的⼀个⽤处，是扩展某个函数的参数，引⼊其他操作。

（2）扩展运算符

扩展运算符（...）⽤于取出参数对象的所有可遍历属性，拷⻉到当前对象之中。

扩展运算符可以⽤于合并两个对象。

如果⽤户⾃定义的属性，放在扩展运算符后⾯，则扩展运算符内部的同名属性会被覆盖掉。

如果扩展运算符后⾯是⼀个空对象，则没有任何效果。

如果扩展运算符的参数是null 或undefined，这两个值会被忽略，不会报错。

###### Null传导运算符

编程实务中，如果读取对象内部的某个属性，往往需要判断⼀下该对象是否存在。⽐如，要读取message.body.user.firstName，安全
的写法是写成下⾯这样。
const firstName = (message
&& message.body
129
&& message.body.user
&& message.body.user.firstName) || 'default';
这样的层层判断⾮常麻烦，因此现在有⼀个提案，引⼊了“Null 传导运算符”（null propagation operator）?.，简化上⾯的写法。
const firstName = message?.body?.user?.firstName || 'default';
上⾯代码有三个?. 运算符，只要其中⼀个返回null 或undefined，就不再往下运算，⽽是返回undefined。
“Null 传导运算符” 有四种⽤法。
• obj?.prop // 读取对象属性
• obj?.[expr] // 同上
• func?.(...args) // 函数或对象⽅法的调⽤
• new C?.(...args) // 构造函数的调⽤
传导运算符之所以写成obj?.prop，⽽不是obj?prop，是为了⽅便编译器能够区分三元运算符?:（⽐如obj?prop:123）。

##### Symbol

ES5 的对象属性名都是字符串，这容易造成属性名的冲突。⽐如，你使⽤了⼀个他⼈提供的对象，但⼜想为这个对象添加新的⽅法（mixin 模式），新⽅法的名字就有可能与现有⽅法产⽣冲突。如果有⼀种机制，保证每个属性的名字都是独⼀⽆⼆的就好了，这样就从根本上防⽌属性名的冲突。这
就是ES6 引⼊Symbol 的原因。
ES6 引⼊了⼀种新的原始数据类型Symbol，表示独⼀⽆⼆的值。它是JavaScript 语⾔的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。
Symbol 值通过Symbol 函数⽣成。这就是说，对象的属性名现在可以有两种类型，⼀种是原来就有的字符串，另⼀种就是新增的Symbol 类型。凡是属性名属于Symbol 类型，就都是独⼀⽆⼆的，可以保证不会与其他属性名产⽣冲突。
let s = Symbol();
typeof s
// "symbol"
上⾯代码中，变量s 就是⼀个独⼀⽆⼆的值。typeof 运算符的结果，表明变量s 是Symbol 数据类型，⽽不是字符串之类的其他类型。
注意，Symbol 函数前不能使⽤new 命令，否则会报错。这是因为⽣成的Symbol 是⼀个原始类型的值，不是对象。也就是说，由于Symbol值不是对象，所以不能添加属性。基本上，它是⼀种类似于字符串的数据类型。
Symbol 函数可以接受⼀个字符串作为参数，表示对Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，⽐较容易区分。
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s2 // Symbol(bar)
s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
上⾯代码中，s1 和s2 是两个Symbol 值。如果不加参数，它们在控制台的输出都是Symbol()，不利于区分。有了参数以后，就等于为它
们加上了描述，输出的时候就能够分清，到底是哪⼀个值。
如果Symbol 的参数是⼀个对象，就会调⽤该对象的toString ⽅法，将其转为字符串，然后才⽣成⼀个Symbol 值。
const obj = {
	toString() {
		return 'abc';
	}
};
const sym = Symbol(obj);
sym // Symbol(abc)
注意，Symbol 函数的参数只是表示对当前Symbol 值的描述，因此相同参数的Symbol 函数的返回值是不相等的。
// 没有参数的情况
let s1 = Symbol();
let s2 = Symbol();
s1 === s2 // false
// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');
s1 === s2 // false
上⾯代码中，s1 和s2 都是Symbol 函数的返回值，⽽且参数相同，但是它们是不相等的。
Symbol 值不能与其他类型的值进⾏运算，会报错。

但是，Symbol 值可以显式转为字符串。

另外，Symbol 值也可以转为布尔值，但是不能转为数值。

###### 作为属性名的Symbol

由于每⼀个Symbol 值都是不相等的，这意味着Symbol 值可以作为标识符，⽤于对象的属性名，就能保证不会出现同名的属性。这对于⼀个对象由多个模块构成的情况⾮常有⽤，能防⽌某⼀个键被不⼩⼼改写或覆盖。

注意，Symbol 值作为对象属性名时，不能⽤点运算符。

同理，在对象的内部，使⽤Symbol 值定义属性时，Symbol 值必须放在⽅括号之中。

Symbol 类型还可以⽤于定义⼀组常量，保证这组常量的值都是不相等的。

常量使⽤Symbol 值最⼤的好处，就是其他任何值都不可能有相同的值了，因此可以保证上⾯的switch 语句会按设计的⽅式⼯作。
还有⼀点需要注意，Symbol 值作为属性名时，该属性还是公开属性，不是私有属性。

###### 消除魔术字符串

魔术字符串指的是，在代码之中多次出现、与代码形成强耦合的某⼀个具体的字符串或者数值。⻛格良好的代码，应该尽量消除魔术字符串，改由含义清晰的变量代替。

常⽤的消除魔术字符串的⽅法，就是把它写成⼀个变量。

###### 属性名的遍历

Symbol 作为属性名，该属性不会出现在for...in、for...of 循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify() 返回。但是，它也不是私有属性，有⼀个Object.getOwnPropertySymbols ⽅法，可以获取指定对象的所有Symbol 属性名。

Object.getOwnPropertySymbols ⽅法返回⼀个数组，成员是当前对象的所有⽤作属性名的Symbol 值。

另⼀个新的API，Reflect.ownKeys ⽅法可以返回所有类型的键名，包括常规键名和Symbol 键名。

由于以Symbol 值作为名称的属性，不会被常规⽅法遍历得到。我们可以利⽤这个特性，为对象定义⼀些⾮私有的、但⼜希望只⽤于内部的⽅法。

###### Symbol.for()，Symbol.keyFor()

有时，我们希望重新使⽤同⼀个Symbol 值，Symbol.for ⽅法可以做到这⼀点。它接受⼀个字符串作为参数，然后搜索有没有以该参数作为名称的Symbol 值。如果有，就返回这个Symbol 值，否则就新建并返回⼀个以该字符串为名称的Symbol 值。

Symbol.for() 与Symbol() 这两种写法，都会⽣成新的Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。Symbol.for() 不会每次调⽤就返回⼀个新的Symbol 类型的值，⽽是会先检查给定的key 是否已经存在，如果不存在才会新建⼀个值。⽐如，如果你调⽤Symbol.for("cat")30 次，每次都会返回同⼀个Symbol 值，但是调⽤Symbol("cat")30 次，会返回30个不同的Symbol 值。

Symbol.keyFor ⽅法返回⼀个已登记的Symbol 类型值的key。

需要注意的是，Symbol.for 为Symbol 值登记的名字，是全局环境的，可以在不同的iframe 或service worker 中取到同⼀个值。

###### 实例：模块的Singleton 模式

Singleton 模式指的是调⽤⼀个类，任何时候返回的都是同⼀个实例。

###### 内置的Symbol值

除了定义⾃⼰使⽤的Symbol 值以外，ES6 还提供了11 个内置的Symbol 值，指向语⾔内部使⽤的⽅法。

Symbol.hasInstance

对象的Symbol.hasInstance 属性，指向⼀个内部⽅法。当其他对象使⽤instanceof 运算符，判断是否为该对象的实例时，会调⽤这个⽅法。⽐如，foo instanceof Foo 在语⾔内部，实际调⽤的是Foo[Symbol.hasInstance](foo)。

Symbol.isConcatSpreadable

对象的Symbol.isConcatSpreadable 属性等于⼀个布尔值，表示该对象⽤于Array.prototype.concat() 时，是否可以展开。

Symbol.species

对象的Symbol.species 属性，指向当前对象的构造函数。创造实例时，默认会调⽤这个⽅法，即使⽤这个属性返回的函数当作构造函数，来创造新的实例对象。

Symbol.match

对象的Symbol.match 属性，指向⼀个函数。当执⾏str.match(myObject) 时，如果该属性存在，会调⽤它，返回该⽅法的返回值。

Symbol.replace

对象的Symbol.replace 属性，指向⼀个⽅法，当该对象被String.prototype.replace ⽅法调⽤时，会返回该⽅法的返回值。

Symbol.search

对象的Symbol.search 属性，指向⼀个⽅法，当该对象被String.prototype.search ⽅法调⽤时，会返回该⽅法的返回值。

Symbol.split

对象的Symbol.split 属性，指向⼀个⽅法，当该对象被String.prototype.split ⽅法调⽤时，会返回该⽅法的返回值。

Symbol.iterator

对象的Symbol.iterator 属性，指向该对象的默认遍历器⽅法。

Symbol.toPrimitive

对象的Symbol.toPrimitive 属性，指向⼀个⽅法。该对象被转为原始类型的值时，会调⽤这个⽅法，返回该对象对应的原始类型值。

Symbol.toStringTag

对象的Symbol.toStringTag 属性，指向⼀个⽅法。在该对象上⾯调⽤Object.prototype.toString ⽅法时，如果这个属性存在，它的返回值会出现在toString ⽅法返回的字符串之中，表示对象的类型。也就是说，这个属性可以⽤来定制[object Object]或[object Array] 中object 后⾯的那个字符串。

Symbol.unscopables

对象的Symbol.unscopables 属性，指向⼀个对象。该对象指定了使⽤with 关键字时，哪些属性会被with 环境排除。

##### Set和Map数据结构

###### Set

ES6 提供了新的数据结构Set。它类似于数组，但是成员的值都是唯⼀的，没有重复的值。
Set 本身是⼀个构造函数，⽤来⽣成Set 数据结构。

```
const s = new Set();
[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
for (let i of s) {
	console.log(i);
}
// 2 3 5 4
```

上⾯代码通过add ⽅法向Set 结构加⼊成员，结果表明Set 结构不会添加重复的值。
Set 函数可以接受⼀个数组（或者具有iterable 接⼝的其他数据结构）作为参数，⽤来初始化。

上⾯代码中，也展示了⼀种去除数组重复成员的⽅法。

```
// 去除数组的重复成员
[...new Set(array)]
```

向Set 加⼊值的时候，不会发⽣类型转换，所以5 和"5" 是两个不同的值。Set 内部判断两个值是否不同，使⽤的算法叫做“Same-valueequality”，它类似于精确相等运算符（===），主要的区别是NaN 等于⾃身，⽽精确相等运算符认为NaN 不等于⾃身。

```
let set = new Set();
let a = NaN;
let b = NaN;
set.add(a);
set.add(b);
set // Set {NaN}
```

上⾯代码向Set 实例添加了两个NaN，但是只能加⼊⼀个。这表明，在Set 内部，两个NaN 是相等。
另外，两个对象总是不相等的。

###### Set 实例的属性和方法

Set 结构的实例有以下属性。
• Set.prototype.constructor：构造函数，默认就是Set 函数。
• Set.prototype.size：返回Set 实例的成员总数。
Set 实例的⽅法分为两⼤类：操作⽅法（⽤于操作数据）和遍历⽅法（⽤于遍历成员）。下⾯先介绍四个操作⽅法。
• add(value)：添加某个值，返回Set 结构本身。
• delete(value)：删除某个值，返回⼀个布尔值，表示删除是否成功。
• has(value)：返回⼀个布尔值，表示该值是否为Set 的成员。
• clear()：清除所有成员，没有返回值。

Array.from ⽅法可以将Set 结构转为数组。

```
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```

这就提供了去除数组重复成员的另⼀种⽅法。

```
function dedupe(array) {
	return Array.from(new Set(array));
}
dedupe([1, 1, 2, 3]) // [1, 2, 3]
```

###### 遍历操作

Set 结构的实例有四个遍历⽅法，可以⽤于遍历成员。
• keys()：返回键名的遍历器
• values()：返回键值的遍历器
• entries()：返回键值对的遍历器
• forEach()：使⽤回调函数遍历每个成员
需要特别指出的是，Set 的遍历顺序就是插⼊顺序。这个特性有时⾮常有⽤，⽐如使⽤Set 保存⼀个回调函数列表，调⽤时就能保证按照添加顺
序调⽤。
(1)keys()，values()，entries()
keys ⽅法、values ⽅法、entries ⽅法返回的都是遍历器对象（详⻅《Iterator 对象》⼀章）。由于Set 结构没有键名，只有键值（或者说键名和键值是同⼀个值），所以keys ⽅法和values ⽅法的⾏为完全⼀致。

Set 结构的实例默认可遍历，它的默认遍历器⽣成函数就是它的values ⽅法。

这意味着，可以省略values ⽅法，直接⽤for...of 循环遍历Set。

(2)forEach()

Set 结构的实例与数组⼀样，也拥有forEach ⽅法，⽤于对每个成员执⾏某种操作，没有返回值。

(3)遍历的应用

扩展运算符（...）内部使⽤for...of 循环，所以也可以⽤于Set 结构。

###### WeakSet

WeakSet 结构与Set 类似，也是不重复的值的集合。但是，它与Set 有两个区别。

⾸先，WeakSet 的成员只能是对象，⽽不能是其他类型的值。

```
const ws = new WeakSet();
ws.add(1)
// TypeError: Invalid value used in weak set
ws.add(Symbol())
// TypeError: invalid value used in weak set
```

其次，WeakSet 中的对象都是弱引⽤，即垃圾回收机制不考虑WeakSet 对该对象的引⽤，也就是说，如果其他对象都不再引⽤该对象，那么垃圾回收机制会⾃动回收该对象所占⽤的内存，不考虑该对象还存在于WeakSet 之中。
这是因为垃圾回收机制依赖引⽤计数，如果⼀个值的引⽤次数不为0，垃圾回收机制就不会释放这块内存。结束使⽤该值之后，有时会忘记取消引⽤，导致内存⽆法释放，进⽽可能会引发内存泄漏。WeakSet ⾥⾯的引⽤，都不计⼊垃圾回收机制，所以就不存在这个问题。因此，WeakSet 适合临时存放⼀组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在WeakSet ⾥⾯的引⽤就会⾃动消失。
由于上⾯这个特点，WeakSet 的成员是不适合引⽤的，因为它会随时消失。另外，由于WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运⾏，运⾏前后很可能成员个数是不⼀样的，⽽垃圾回收机制何时运⾏是不可预测的，因此ES6 规定WeakSet 不可遍历。

###### 语法

WeakSet 是⼀个构造函数，可以使⽤new 命令，创建WeakSet 数据结构。

```
const ws = new WeakSet();
```

作为构造函数，WeakSet 可以接受⼀个数组或类似数组的对象作为参数。（实际上，任何具有Iterable 接⼝的对象，都可以作为WeakSet的参数。）该数组的所有成员，都会⾃动成为WeakSet 实例对象的成员。

WeakSet 结构有以下三个⽅法。
• WeakSet.prototype.add(value)：向WeakSet 实例添加⼀个新成员。
• WeakSet.prototype.delete(value)：清除WeakSet 实例的指定成员。
• WeakSet.prototype.has(value)：返回⼀个布尔值，表示某个值是否在WeakSet 实例之中。

WeakSet 没有size 属性，没有办法遍历它的成员。

WeakSet 不能遍历，是因为成员都是弱引⽤，随时可能消失，遍历机制⽆法保证成员的存在，很可能刚刚遍历结束，成员就取不到了。WeakSet的⼀个⽤处，是储存DOM 节点，⽽不⽤担⼼这些节点从⽂档移除时，会引发内存泄漏。

###### Map

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能⽤字符串当作键。这给它的使⽤带来了很⼤的限制。

为了解决这个问题，ES6 提供了Map 数据结构。它类似于对象，也是键值对的集合，但是“键” 的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值” 的对应，Map 结构提供了“值—值” 的对应，是⼀种更完善的Hash 结构实现。如果你需要“键值对” 的数据结构，Map ⽐Object 更合适。

事实上，不仅仅是数组，任何具有Iterator 接⼝、且每个成员都是⼀个双元素的数组的数据结构（详⻅《Iterator》⼀章）都可以当作Map构造函数的参数。这就是说，Set 和Map 都可以⽤来⽣成新的Map。

如果对同⼀个键多次赋值，后⾯的值将覆盖前⾯的值。

如果读取⼀个未知的键，则返回undefined。

Map 的键实际上是跟内存地址绑定的，只要内存地址不⼀样，就视为两个键。这就解决了同名属性碰撞（clash）的问题，我们扩展别⼈的库的时候，如果使⽤对象作为键名，就不⽤担⼼⾃⼰的属性与原作者的属性同名。
如果Map 的键是⼀个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map 将其视为⼀个键，⽐如0 和-0 就是⼀个键，布尔值true 和字符串true 则是两个不同的键。另外，undefined 和null 也是两个不同的键。虽然NaN 不严格相等于⾃身，但Map将其视为同⼀个键。

###### 实例的属性和操作⽅法

(1)size属性

size属性返回Map结构的成员总数。

(2)set(key, value)

set ⽅法设置键名key 对应的键值为value，然后返回整个Map 结构。如果key 已经有值，则键值会被更新，否则就新⽣成该键。

set方法返回的是当前的Map对象，因此可以采用链式写法。

(3)get(key)

get ⽅法读取key 对应的键值，如果找不到key，返回undefined。

(4)has(key)

has ⽅法返回⼀个布尔值，表示某个键是否在当前Map 对象之中。

(5)delete(key)

delete ⽅法删除某个键，返回true。如果删除失败，返回false。

(6)clear()

clear ⽅法清除所有成员，没有返回值。

###### 遍历方法

Map 结构原⽣提供三个遍历器⽣成函数和⼀个遍历⽅法。

• keys()：返回键名的遍历器。
• values()：返回键值的遍历器。
• entries()：返回所有成员的遍历器。
• forEach()：遍历Map 的所有成员。
需要特别注意的是，Map 的遍历顺序就是插⼊顺序。

Map 结构转为数组结构，⽐较快速的⽅法是使⽤扩展运算符（...）。

结合数组的map ⽅法、filter ⽅法，可以实现Map 的遍历和过滤（Map 本身没有map 和filter ⽅法）。

forEach ⽅法还可以接受第⼆个参数，⽤来绑定this。

###### 与其他数据结构的互相转换

(1)Map转为数组

Map 转为数组最⽅便的⽅法，就是使⽤扩展运算符（...）。

(2)数组转为Map

将数组传⼊Map 构造函数，就可以转为Map。

(3)Map转为对象

如果所有Map 的键都是字符串，它可以转为对象。

(4)对象转为Map

(5)Map转为JSON

Map 转为JSON 要区分两种情况。⼀种情况是，Map 的键名都是字符串，这时可以选择转为对象JSON。

另⼀种情况是，Map 的键名有⾮字符串，这时可以选择转为数组JSON。

(6)JSON转为Map

JSON 转为Map，正常情况下，所有键名都是字符串。

但是，有⼀种特殊情况，整个JSON 就是⼀个数组，且每个数组成员本身，⼜是⼀个有两个成员的数组。这时，它可以⼀⼀对应地转为Map。这往往是数组转为JSON 的逆操作。

###### WeakMap

WeakMap 结构与Map 结构类似，也是⽤于⽣成键值对的集合。

WeakMap 与Map 的区别有两点。
⾸先，WeakMap 只接受对象作为键名（null 除外），不接受其他类型的值作为键名。

其次，WeakMap 的键名所指向的对象，不计⼊垃圾回收机制。

WeakMap 的设计⽬的在于，有时我们想在某个对象上⾯存放⼀些数据，但是这会形成对于这个对象的引⽤。

WeakMap 就是为了解决这个问题⽽诞⽣的，它的键名所引⽤的对象都是弱引⽤，即垃圾回收机制不将该引⽤考虑在内。因此，只要所引⽤的对象的其他引⽤都被清除，垃圾回收机制就会释放该对象所占⽤的内存。也就是说，⼀旦不再需要，WeakMap ⾥⾯的键名对象和所对应的键值对会⾃动消失，不⽤⼿动删除引⽤。
基本上，如果你要往对象上添加数据，⼜不想⼲扰垃圾回收机制，就可以使⽤WeakMap。⼀个典型应⽤场景是，在⽹⻚的DOM 元素上添加数据，就可以使⽤WeakMap 结构。当该DOM 元素被清除，其所对应的WeakMap 记录就会⾃动被移除。

总之，WeakMap 的专⽤场合就是，它的键所对应的对象，可能会在将来消失。WeakMap 结构有助于防⽌内存泄漏。
注意，WeakMap 弱引⽤的只是键名，⽽不是键值。键值依然是正常引⽤。

###### WeakMap的语法

WeakMap 与Map 在API 上的区别主要是两个，⼀是没有遍历操作（即没有key()、values() 和entries() ⽅法），也没有size属性。因为没有办法列出所有键名，某个键名是否存在完全不可预测，跟垃圾回收机制是否运⾏相关。这⼀刻可以取到键名，下⼀刻垃圾回收机制突然运⾏了，这个键名就没了，为了防⽌出现不确定性，就统⼀规定不能取到键名。⼆是⽆法清空，即不⽀持clear ⽅法。因此，WeakMap 只有四个⽅法可⽤：get()、set()、has()、delete()。

##### Proxy

Proxy ⽤于修改某些操作的默认⾏为，等同于在语⾔层⾯做出修改，所以属于⼀种“元编程”（meta programming），即对编程语⾔进⾏编程。
Proxy 可以理解成，在⽬标对象之前架设⼀层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了⼀种机制，可以对外界的访问进⾏过滤和改写。Proxy 这个词的原意是代理，⽤在这⾥表示由它来“代理” 某些操作，可以译为“代理器”。

ES6 原⽣提供Proxy 构造函数，⽤来⽣成Proxy 实例。
var proxy = new Proxy(target, handler);
Proxy 对象的所有⽤法，都是上⾯这种形式，不同的只是handler 参数的写法。其中，new Proxy() 表示⽣成⼀个Proxy 实例，target 参数表示所要拦截的⽬标对象，handler 参数也是⼀个对象，⽤来定制拦截⾏为。

注意，要使得Proxy 起作⽤，必须针对Proxy 实例（上例是proxy 对象）进⾏操作，⽽不是针对⽬标对象（上例是空对象）进⾏操作。
如果handler 没有设置任何拦截，那就等同于直接通向原对象。

下⾯是Proxy ⽀持的拦截操作⼀览，⼀共13 种。
• get(target, propKey, receiver)：拦截对象属性的读取，⽐如proxy.foo 和proxy['foo']。
• set(target, propKey, value, receiver)：拦截对象属性的设置，⽐如proxy.foo = v 或proxy['foo'] = v，
返回⼀个布尔值。
• has(target, propKey)：拦截propKey in proxy 的操作，返回⼀个布尔值。
• deleteProperty(target, propKey)：拦截delete proxy[propKey] 的操作，返回⼀个布尔值。
• ownKeys(target)：拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、
Object.keys(proxy)，返回⼀个数组。该⽅法返回⽬标对象所有⾃身的属性的属性名，⽽Object.keys() 的返回结果仅包括⽬标对象⾃身的可遍历属性。
• getOwnPropertyDescriptor(target, propKey)： 拦截Object.getOwnPropertyDescriptor(proxy,
propKey)，返回属性的描述对象。
• defineProperty(target, propKey, propDesc)： 拦截Object.defineProperty(proxy, propKey,
propDesc）、Object.defineProperties(proxy, propDescs)，返回⼀个布尔值。
• preventExtensions(target)：拦截Object.preventExtensions(proxy)，返回⼀个布尔值。
• getPrototypeOf(target)：拦截Object.getPrototypeOf(proxy)，返回⼀个对象。
• isExtensible(target)：拦截Object.isExtensible(proxy)，返回⼀个布尔值。
• setPrototypeOf(target, proto)：拦截Object.setPrototypeOf(proxy, proto)，返回⼀个布尔值。如果⽬标
对象是函数，那么还有两种额外操作可以拦截。
• apply(target, object, args)：拦截Proxy 实例作为函数调⽤的操作，⽐如proxy(...args)、proxy.call(object,
...args)、proxy.apply(...)。
• construct(target, args)：拦截Proxy 实例作为构造函数调⽤的操作，⽐如new proxy(...args)

###### Proxy 实例的⽅法

###### get()

get ⽅法⽤于拦截某个属性的读取操作，可以接受三个参数，依次为⽬标对象、属性名和proxy 实例本身（即this 关键字指向的那个对象），其中最后⼀个参数可选。

###### set()

set ⽅法⽤来拦截某个属性的赋值操作，可以接受四个参数，依次为⽬标对象、属性名、属性值和Proxy 实例本身，其中最后⼀个参数可选。

###### apply()

apply ⽅法拦截函数的调⽤、call 和apply 操作。
apply ⽅法可以接受三个参数，分别是⽬标对象、⽬标对象的上下⽂对象（this）和⽬标对象的参数数组。

###### has()

has ⽅法⽤来拦截HasProperty 操作，即判断对象是否具有某个属性时，这个⽅法会⽣效。典型的操作就是in 运算符。

###### construct()

construct ⽅法⽤于拦截new 命令

###### deleteProperty()

deleteProperty ⽅法⽤于拦截delete 操作，如果这个⽅法抛出错误或者返回false，当前属性就⽆法被delete 命令删除。

###### defineProperty()

defineProperty ⽅法拦截了Object.defineProperty 操作。

###### getOwnPropertyDescriptor()

getOwnPropertyDescriptor ⽅法拦截Object.getOwnPropertyDescriptor()， 返回⼀个属性描述对象或者
undefined。

###### getPrototypeOf()

getPrototypeOf ⽅法主要⽤来拦截获取对象原型。

###### isExtensible()

isExtensible ⽅法拦截Object.isExtensible 操作。

###### ownKeys()

ownKeys ⽅法⽤来拦截对象⾃身属性的读取操作。

###### preventExtensions()

preventExtensions ⽅法拦截Object.preventExtensions()。该⽅法必须返回⼀个布尔值，否则会被⾃动转为布尔值。
这个⽅法有⼀个限制，只有⽬标对象不可扩展时（即Object.isExtensible(proxy) 为false），proxy.preventExtensions 才能返回true，否则会报错。

###### setPrototypeOf()

setPrototypeOf ⽅法主要⽤来拦截Object.setPrototypeOf ⽅法。

###### Proxy.revocable()

Proxy.revocable ⽅法返回⼀个可取消的Proxy 实例。

###### this问题

虽然Proxy 可以代理针对⽬标对象的访问，但它不是⽬标对象的透明代理，即不做任何拦截的情况下，也⽆法保证与⽬标对象的⾏为⼀致。主要原因就是在Proxy 代理的情况下，⽬标对象内部的this 关键字会指向Proxy 代理。

##### Reflect

Reflect 对象与Proxy 对象⼀样，也是ES6 为了操作对象⽽提供的新API。Reflect 对象的设计⽬的有这样⼏个。

（1）将Object 对象的⼀些明显属于语⾔内部的⽅法（⽐如Object.defineProperty），放到Reflect 对象上。现阶段，某些⽅法同时在Object 和Reflect 对象上部署，未来的新⽅法将只部署在Reflect 对象上。也就是说，从Reflect 对象上可以拿到语⾔内部的⽅法。
（2）修改某些Object ⽅法的返回结果，让其变得更合理。⽐如，Object.defineProperty(obj, name, desc) 在⽆法定义属性时，会抛出⼀个错误，⽽Reflect.defineProperty(obj, name, desc) 则会返回false。

（3）让Object 操作都变成函数⾏为。某些Object 操作是命令式， ⽐如name in obj 和delete obj[name]， ⽽
Reflect.has(obj, name) 和Reflect.deleteProperty(obj, name) 让它们变成了函数⾏为。

（4）Reflect 对象的⽅法与Proxy 对象的⽅法⼀⼀对应，只要是Proxy 对象的⽅法，就能在Reflect 对象上找到对应的⽅法。这就让Proxy 对象可以⽅便地调⽤对应的Reflect ⽅法，完成默认⾏为，作为修改⾏为的基础。也就是说，不管Proxy 怎么修改默认⾏为，你总可以在Reflect 上获取默认⾏为。

###### 静态方法

Reflect 对象⼀共有13 个静态⽅法。
• Reflect.apply(target, thisArg, args)
• Reflect.construct(target, args)
• Reflect.get(target, name, receiver)
• Reflect.set(target, name, value, receiver)
• Reflect.defineProperty(target, name, desc)
• Reflect.deleteProperty(target, name)
• Reflect.has(target, name)
• Reflect.ownKeys(target)
• Reflect.isExtensible(target)
• Reflect.preventExtensions(target)
• Reflect.getOwnPropertyDescriptor(target, name)
• Reflect.getPrototypeOf(target)
• Reflect.setPrototypeOf(target, prototype)
上⾯这些⽅法的作⽤，⼤部分与Object 对象的同名⽅法的作⽤都是相同的，⽽且它与Proxy 对象的⽅法是⼀⼀对应的。下⾯是对它们的解
释。

###### Reflect.get(target, name, receiver)

Reflect.get ⽅法查找并返回target 对象的name 属性，如果没有该属性，则返回undefined。

如果name 属性部署了读取函数（getter），则读取函数的this 绑定receiver。

如果第⼀个参数不是对象，Reflect.get ⽅法会报错。

###### Reflect.set(target, name, value, receiver)

Reflect.set ⽅法设置target 对象的name 属性等于value。

如果name 属性设置了赋值函数，则赋值函数的this 绑定receiver。

注意，如果Proxy 对象和Reflect 对象联合使⽤，前者拦截赋值操作，后者完成赋值的默认⾏为，⽽且传⼊了receiver，那么Reflect.set 会触发Proxy.defineProperty 拦截。

如果第⼀个参数不是对象，Reflect.set 会报错。

###### Reflect.has(obj, name)

Reflect.has ⽅法对应name in obj ⾥⾯的in 运算符。

如果第⼀个参数不是对象，Reflect.has 和in 运算符都会报错。

###### Reflect.deleteProperty(obj, name)

Reflect.deleteProperty ⽅法等同于delete obj[name]，⽤于删除对象的属性。

该⽅法返回⼀个布尔值。如果删除成功，或者被删除的属性不存在，返回true；删除失败，被删除的属性依然存在，返回false。

###### Reflect.construct(target, args)

Reflect.construct ⽅法等同于new target(...args)，这提供了⼀种不使⽤new，来调⽤构造函数的⽅法。

###### Reflect.getPrototypeOf(obj)

Reflect.getPrototypeOf ⽅法⽤于读取对象的__proto__ 属性，对应Object.getPrototypeOf(obj)。

Reflect.getPrototypeOf 和Object.getPrototypeOf 的⼀个区别是，如果参数不是对象，Object.getPrototypeOf
会将这个参数转为对象，然后再运⾏，⽽Reflect.getPrototypeOf 会报错。

###### Reflect.setPrototypeOf(obj, newProto)

Reflect.setPrototypeOf ⽅法⽤于设置对象的__proto__ 属性，返回第⼀个参数对象，对应Object.setPrototypeOf(obj,newProto)。

如果第⼀个参数不是对象，Object.setPrototypeOf 会返回第⼀个参数本身，⽽Reflect.setPrototypeOf 会报错。

如果第⼀个参数是undefined 或null，Object.setPrototypeOf 和Reflect.setPrototypeOf 都会报错。

###### Reflect.apply(func, thisArg, args)

Reflect.apply ⽅法等同于Function.prototype.apply.call(func, thisArg, args)，⽤于绑定this 对象后
执⾏给定函数。
⼀般来说，如果要绑定⼀个函数的this 对象，可以这样写fn.apply(obj, args)，但是如果函数定义了⾃⼰的apply ⽅法，就只能写成Function.prototype.apply.call(fn, obj, args)，采⽤Reflect 对象可以简化这种操作。

###### Reflect.defineProperty(target, propertyKey, attributes)

Reflect.defineProperty ⽅法基本等同于Object.defineProperty，⽤来为对象定义属性。未来，后者会被逐渐废除，请从现在开始就使⽤Reflect.defineProperty 代替它。

⼀般来说，如果要绑定⼀个函数的this 对象，可以这样写fn.apply(obj, args)，但是如果函数定义了⾃⼰的apply ⽅法，就只能写成Function.prototype.apply.call(fn, obj, args)，采⽤Reflect 对象可以简化这种操作。

###### Reflect.defineProperty(target, propertyKey, attributes)

Reflect.defineProperty ⽅法基本等同于Object.defineProperty，⽤来为对象定义属性。未来，后者会被逐渐废除，请从现在开始就使⽤Reflect.defineProperty 代替它。

如果Reflect.defineProperty 的第⼀个参数不是对象，就会抛出错误，⽐如Reflect.defineProperty(1, 'foo')。

###### Reflect.getOwnPropertyDescriptor(target, propertyKey)

Reflect.getOwnPropertyDescriptor 基本等同于Object.getOwnPropertyDescriptor，⽤于得到指定属性的描述对象，将来会替代掉后者。

###### Reflect.isExtensible (target)

Reflect.isExtensible ⽅法对应Object.isExtensible，返回⼀个布尔值，表示当前对象是否可扩展。

如果参数不是对象，Object.isExtensible 会返回false，因为⾮对象本来就是不可扩展的，⽽Reflect.isExtensible 会报错。

###### Reflect.preventExtensions(target)

Reflect.preventExtensions 对应Object.preventExtensions ⽅法，⽤于让⼀个对象变为不可扩展。它返回⼀个布尔值，表示是否操作成功。

如果参数不是对象，Object.preventExtensions 在ES5 环境报错，在ES6 环境返回传⼊的参数，⽽Reflect.preventExtensions会报错。

###### Reflect.ownKeys (target)

Reflect.ownKeys ⽅法⽤于返回对象的所有属性，基本等同于Object.getOwnPropertyNames 与Object.getOwnPropertySymbols之和。

观察者模式（Observer mode）指的是函数自动观察数据对象，一旦对象有变化，函数就会自动执行。

##### Promise

Promise 是异步编程的⼀种解决⽅案，⽐传统的解决⽅案——回调函数和事件——更合理和更强⼤。它由社区最早提出和实现，ES6 将其写进了语⾔标准，统⼀了⽤法，原⽣提供了Promise 对象。
所谓Promise，简单说就是⼀个容器，⾥⾯保存着某个未来才会结束的事件（通常是⼀个异步操作）的结果。从语法上说，Promise 是⼀个对象，从它可以获取异步操作的消息。Promise 提供统⼀的API，各种异步操作都可以⽤同样的⽅法进⾏处理。
Promise 对象有以下两个特点。
（1）对象的状态不受外界影响。Promise 对象代表⼀个异步操作，有三种状态：pending（进⾏中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪⼀种状态，任何其他操作都⽆法改变这个状态。这也是Promise 这个名字的由来，它的英语意思就是“承诺”，表示其他⼿段⽆法改变。
（2）⼀旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从pending 变为fulfilled和从pending 变为rejected。只要这两种情况发⽣，状态就凝固了，不会再变了，会⼀直保持这个结果，这时就称为resolved（已定型）。如果改变已经发⽣了，你再对Promise 对象添加回调函数，也会⽴即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。
注意，为了⾏⽂⽅便，本章后⾯的resolved 统⼀只指fulfilled 状态，不包含rejected 状态。

有了Promise 对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise 对象提供统⼀的接⼝，使得控制异步操作更加容易。
Promise 也有⼀些缺点。⾸先，⽆法取消Promise，⼀旦新建它就会⽴即执⾏，⽆法中途取消。其次，如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。第三，当处于pending 状态时，⽆法得知⽬前进展到哪⼀个阶段（刚刚开始还是即将完成）。
如果某些事件不断地反复发⽣，⼀般来说，使⽤Stream 模式是⽐部署Promise 更好的选择。

###### 基本用法

ES6规定，Promise对象是一个构造函数，用来生成Promise实例。

```
var promise = new Promise(function(resolve, reject){
	//... some code
	if(/*异步操作成功*/){
		resolve(value);
	}else{
		reject(error);
	}
});
```

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个参数，由JavaScript引擎提供，不用自己部署。

resolve函数的作用是，将Promise对象的状态从‘未完成’变为“成功”（即从pending变为resolved）,在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从pending变为rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。

```
promise.then(function(value){
	//success
},function(error){
	//failure
})
```

then ⽅法可以接受两个回调函数作为参数。第⼀个回调函数是Promise 对象的状态变为resolved 时调⽤，第⼆个回调函数是Promise 对象的状态变为rejected 时调⽤。其中，第⼆个函数是可选的，不⼀定要提供。这两个函数都接受Promise 对象传出的值作为参数。

Promise 新建后就会⽴即执⾏。

```
let promise = new Promise(function(resolve, reject) {
	console.log('Promise');
	resolve();
});
promise.then(function() {
	console.log('resolved.');
});
console.log('Hi!');
// Promise
// Hi!
// resolved
```

上⾯代码中，Promise 新建后⽴即执⾏，所以⾸先输出的是Promise。然后，then ⽅法指定的回调函数，将在当前脚本所有同步任务执⾏完才会执⾏，所以resolved 最后输出。

如果调⽤resolve 函数和reject 函数时带有参数，那么它们的参数会被传递给回调函数。reject 函数的参数通常是Error 对象的实例，表示抛出的错误；resolve 函数的参数除了正常的值以外，还可能是另⼀个Promise 实例，

```
var p1 = new Promise(function (resolve, reject) {
// ...
});
var p2 = new Promise(function (resolve, reject) {
// ...
resolve(p1);
})
```

上⾯代码中，p1 和p2 都是Promise 的实例，但是p2 的resolve ⽅法将p1 作为参数，即⼀个异步操作的结果是返回另⼀个异步操作。
注意，这时p1 的状态就会传递给p2，也就是说，p1 的状态决定了p2 的状态。如果p1 的状态是pending，那么p2 的回调函数就会等待p1 的状态改变；如果p1 的状态已经是resolved 或者rejected，那么p2 的回调函数将会⽴刻执⾏。

注意，调⽤resolve 或reject 并不会终结Promise 的参数函数的执⾏。

```
new Promise((resolve, reject) => {
	resolve(1);
	console.log(2);
}).then(r => {
	console.log(r);
});
// 2
// 1
```

上⾯代码中，调⽤resolve(1) 以后，后⾯的console.log(2) 还是会执⾏，并且会⾸先打印出来。这是因为⽴即resolved 的Promise 是在本轮事件循环的末尾执⾏，总是晚于本轮循环的同步任务。
⼀般来说，调⽤resolve 或reject 以后，Promise 的使命就完成了，后继操作应该放到then ⽅法⾥⾯，⽽不应该直接写在resolve 或reject 的后⾯。所以，最好在它们前⾯加上return 语句，这样就不会有意外。

```
new Promise((resolve, reject) => {
	return resolve(1);
	// 后⾯的语句不会执⾏
	console.log(2);
})
```

###### Promise.prototype.then()

Promise 实例具有then ⽅法，也就是说，then ⽅法是定义在原型对象Promise.prototype 上的。它的作⽤是为Promise 实例添加状态改变时的回调函数。前⾯说过，then ⽅法的第⼀个参数是resolved 状态的回调函数，第⼆个参数（可选）是rejected 状态的回调函数。
then ⽅法返回的是⼀个新的Promise 实例（注意，不是原来那个Promise 实例）。因此可以采⽤链式写法，即then ⽅法后⾯再调⽤另⼀个then ⽅法。

```
getJSON("/posts.json").then(function(json) {
	return json.post;
}).then(function(post) {
	// ...
});
```

上⾯的代码使⽤then ⽅法，依次指定了两个回调函数。第⼀个回调函数完成以后，会将返回结果作为参数，传⼊第⼆个回调函数。
采⽤链式的then，可以指定⼀组按照次序调⽤的回调函数。这时，前⼀个回调函数，有可能返回的还是⼀个Promise 对象（即有异步操作），这时后⼀个回调函数，就会等待该Promise 对象的状态发⽣变化，才会被调⽤。

###### Promise.prototype.catch()

Promise.prototype.catch ⽅法是.then(null, rejection) 的别名，⽤于指定发⽣错误时的回调函数。

```
getJSON('/posts.json').then(function(posts) {
	// ...
}).catch(function(error) {
	// 处理getJSON 和前⼀个回调函数运⾏时发⽣的错误
	console.log(' 发⽣错误！', error);
});
```

上⾯代码中，getJSON ⽅法返回⼀个Promise 对象，如果该对象状态变为resolved，则会调⽤then ⽅法指定的回调函数；如果异步操作抛出错误，状态就会变为rejected，就会调⽤catch ⽅法指定的回调函数，处理这个错误。另外，then ⽅法指定的回调函数，如果运⾏中抛出错误，也会被catch ⽅法捕获。

可以发现reject ⽅法的作⽤，等同于抛出错误。
如果Promise 状态已经变成resolved，再抛出错误是⽆效的。

```
var promise = new Promise(function(resolve, reject) {
	resolve('ok');
	throw new Error('test');
});
promise
.then(function(value) { console.log(value) })
.catch(function(error) { console.log(error) });
// ok
```

上⾯代码中，Promise 在resolve 语句后⾯，再抛出错误，不会被捕获，等于没有抛出。因为Promise 的状态⼀旦改变，就永久保持该状态，不会再变了。
Promise 对象的错误具有“冒泡” 性质，会⼀直向后传递，直到被捕获为⽌。也就是说，错误总是会被下⼀个catch 语句捕获。

⼀般来说，不要在then ⽅法⾥⾯定义Reject 状态的回调函数（即then 的第⼆个参数），总是使⽤catch ⽅法。

```
// bad
promise
.then(function(data) {
 // success
}, function(err) {
 // error
});
// good
promise
.then(function(data) { //cb
 // success
})
.catch(function(err) {
 // error
});
```

上⾯代码中，第⼆种写法要好于第⼀种写法，理由是第⼆种写法可以捕获前⾯then ⽅法执⾏中的错误，也更接近同步的写法（try/catch）。
因此，建议总是使⽤catch ⽅法，⽽不使⽤then ⽅法的第⼆个参数。

跟传统的try/catch 代码块不同的是，如果没有使⽤catch ⽅法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。

Promise 内部的错误不会影响到Promise 外部的代码，通俗的说法就是“Promise 会吃掉错误”。

⼀般总是建议，Promise 对象后⾯要跟catch ⽅法，这样可以处理Promise 内部发⽣的错误。catch ⽅法返回的还是⼀个Promise对象，因此后⾯还可以接着调⽤then ⽅法。

catch ⽅法之中，还能再抛出错误。

```
someAsyncThing().then(function() {
	return someOtherAsyncThing();
}).catch(function(error) {
	console.log('oh no', error);
	// 下⾯⼀⾏会报错，因为y 没有声明
	y + 2;
}).catch(function(error) {
	console.log('carry on', error);
});
// oh no [ReferenceError: x is not defined]
// carry on [ReferenceError: y is not defined]
```

上⾯代码中，第⼆个catch ⽅法⽤来捕获，前⼀个catch ⽅法抛出的错误。

###### Promise.all()

Promise.all ⽅法⽤于将多个Promise 实例，包装成⼀个新的Promise 实例。

```
var p = Promise.all([p1, p2, p3]);
```

上⾯代码中，Promise.all ⽅法接受⼀个数组作为参数，p1、p2、p3 都是Promise 实例，如果不是，就会先调⽤下⾯讲到的Promise.resolve ⽅法，将参数转为Promise 实例，再进⼀步处理。（Promise.all ⽅法的参数可以不是数组，但必须具有Iterator 接⼝，且返回的每个成员都是Promise 实例。）
p 的状态由p1、p2、p3 决定，分成两种情况。
（1）只有p1、p2、p3 的状态都变成fulfilled，p 的状态才会变成fulfilled，此时p1、p2、p3 的返回值组成⼀个数组，传递给p 的回调函数。
（2）只要p1、p2、p3 之中有⼀个被rejected，p 的状态就变成rejected，此时第⼀个被reject 的实例的返回值，会传递给p的回调函数。

###### Promise.race()

Promise.race ⽅法同样是将多个Promise 实例，包装成⼀个新的Promise 实例。

```
var p = Promise.race([p1, p2, p3]);
```

上⾯代码中，只要p1、p2、p3 之中有⼀个实例率先改变状态，p 的状态就跟着改变。那个率先改变的Promise 实例的返回值，就传递给p的回调函数。
Promise.race ⽅法的参数与Promise.all ⽅法⼀样，如果不是Promise 实例，就会先调⽤下⾯讲到的Promise.resolve⽅法，将参数转为Promise 实例，再进⼀步处理。

###### Promise.resolve()

有时需要将现有对象转为Promise 对象，Promise.resolve ⽅法就起到这个作⽤。

```
var jsPromise = Promise.resolve($.ajax('/whatever.json'));
```

上⾯代码将jQuery ⽣成的deferred 对象，转为⼀个新的Promise 对象。
Promise.resolve 等价于下⾯的写法。

```
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

Promise.resolve ⽅法的参数分成四种情况。

（1）参数是⼀个Promise 实例
如果参数是Promise 实例，那么Promise.resolve 将不做任何修改、原封不动地返回这个实例。

（2）参数是⼀个thenable 对象

thenable 对象指的是具有then ⽅法的对象，⽐如下⾯这个对象。

```
let thenable = {
	then: function(resolve, reject) {
		resolve(42);
	}
};
```

Promise.resolve ⽅法会将这个对象转为Promise 对象，然后就⽴即执⾏thenable 对象的then ⽅法。

```
let thenable = {
	then: function(resolve, reject) {
		resolve(42);
	}
};
let p1 = Promise.resolve(thenable);
p1.then(function(value) {
	console.log(value); // 42
});
```

上⾯代码中，thenable 对象的then ⽅法执⾏后，对象p1 的状态就变为resolved，从⽽⽴即执⾏最后那个then ⽅法指定的回调函数，输出42。
（3）参数不是具有then ⽅法的对象，或根本就不是对象

如果参数是⼀个原始值，或者是⼀个不具有then ⽅法的对象，则Promise.resolve ⽅法返回⼀个新的Promise 对象，状态为resolved。

```
var p = Promise.resolve('Hello');
p.then(function (s){
	console.log(s)
});
// Hello
```

上⾯代码⽣成⼀个新的Promise 对象的实例p。由于字符串Hello 不属于异步操作（判断⽅法是字符串对象不具有then ⽅法），返回Promise 实例的状态从⼀⽣成就是resolved，所以回调函数会⽴即执⾏。Promise.resolve ⽅法的参数，会同时传给回调函数。

（4）不带有任何参数

Promise.resolve ⽅法允许调⽤时不带参数，直接返回⼀个resolved 状态的Promise 对象。
所以，如果希望得到⼀个Promise 对象，⽐较⽅便的⽅法就是直接调⽤Promise.resolve ⽅法。

```
var p = Promise.resolve();
p.then(function () {
// ...
});
```

###### Promise.reject()

Promise.reject(reason) ⽅法也会返回⼀个新的Promise 实例，该实例的状态为rejected。

```
var p = Promise.reject(' 出错了');
// 等同于
var p = new Promise((resolve, reject) => reject(' 出错了'))
p.then(null, function (s) {
	console.log(s)
});
// 出错了
```

注意，Promise.reject() ⽅法的参数，会原封不动地作为reject 的理由，变成后续⽅法的参数。这⼀点与Promise.resolve⽅法不⼀致。

###### 两个有⽤的附加⽅法

done()
Promise 对象的回调链，不管以then ⽅法或catch ⽅法结尾，要是最后⼀个⽅法抛出错误，都有可能⽆法捕捉到（因为Promise 内部的错误不会冒泡到全局）。因此，我们可以提供⼀个done ⽅法，总是处于回调链的尾端，保证抛出任何可能出现的错误。

finally()
finally ⽅法⽤于指定不管Promise 对象最后状态如何，都会执⾏的操作。它与done ⽅法的最⼤区别，它接受⼀个普通的回调函数作为参数，该函数不管怎样都必须执⾏。

##### Iterator 和for…of 循环

###### Iterator（遍历器）的概念

JavaScript 原有的表示“集合” 的数据结构，主要是数组（Array）和对象（Object），ES6 ⼜添加了Map 和Set。这样就有了四种数据集合，⽤户还可以组合使⽤它们，定义⾃⼰的数据结构，⽐如数组的成员是Map，Map 的成员是对象。这样就需要⼀种统⼀的接⼝机制，来处理所有不同的数据结构。
遍历器（Iterator）就是这样⼀种机制。它是⼀种接⼝，为各种不同的数据结构提供统⼀的访问机制。任何数据结构只要部署Iterator 接⼝，就可以完成遍历操作（即依次处理该数据结构的所有成员）。
Iterator 的作⽤有三个：⼀是为各种数据结构，提供⼀个统⼀的、简便的访问接⼝；⼆是使得数据结构的成员能够按某种次序排列；三是ES6创造了⼀种新的遍历命令for...of 循环，Iterator 接⼝主要供for...of 消费。
Iterator 的遍历过程是这样的。
（1）创建⼀个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是⼀个指针对象。
（2）第⼀次调⽤指针对象的next ⽅法，可以将指针指向数据结构的第⼀个成员。
（3）第⼆次调⽤指针对象的next ⽅法，指针就指向数据结构的第⼆个成员。
（4）不断调⽤指针对象的next ⽅法，直到它指向数据结构的结束位置。

每⼀次调⽤next ⽅法，都会返回数据结构的当前成员的信息。具体来说，就是返回⼀个包含value 和done 两个属性的对象。其中，value 属性是当前成员的值，done 属性是⼀个布尔值，表示遍历是否结束。

```
var it = makeIterator(['a', 'b']);
it.next() // { value: "a", done: false }
it.next() // { value: "b", done: false }
it.next() // { value: undefined, done: true }
function makeIterator(array) {
	var nextIndex = 0;
	return {
		next: function() {
			return nextIndex < array.length ?
				{value: array[nextIndex++], done: false} :
				{value: undefined, done: true};
		}
	};
}
```

上⾯代码定义了⼀个makeIterator 函数，它是⼀个遍历器⽣成函数，作⽤就是返回⼀个遍历器对象。对数组['a', 'b'] 执⾏这个函数，就会返回该数组的遍历器对象（即指针对象）it。
指针对象的next ⽅法，⽤来移动指针。开始时，指针指向数组的开始位置。然后，每次调⽤next ⽅法，指针就会指向数组的下⼀个成员。第⼀次调⽤，指向a；第⼆次调⽤，指向b。
next ⽅法返回⼀个对象，表示当前数据成员的信息。这个对象具有value 和done 两个属性，value 属性返回当前位置的成员，done属性是⼀个布尔值，表示遍历是否结束，即是否还有必要再⼀次调⽤next ⽅法。
总之，调⽤指针对象的next ⽅法，就可以遍历事先给定的数据结构。

对于遍历器对象来说，done: false 和value: undefined 属性都是可以省略的。

由于Iterator 只是把接⼝规格加到数据结构之上，所以，遍历器与它所遍历的那个数据结构，实际上是分开的，完全可以写出没有对应数据结构的遍历器对象，或者说⽤遍历器对象模拟出数据结构。

###### 默认Iterator 接⼝

Iterator 接⼝的⽬的，就是为所有数据结构，提供了⼀种统⼀的访问机制，即for...of 循环（详⻅下⽂）。当使⽤for...of 循环遍历某种数据结构时，该循环会⾃动去寻找Iterator 接⼝。
⼀种数据结构只要部署了Iterator 接⼝，我们就称这种数据结构是” 可遍历的“（iterable）。

ES6 规定，默认的Iterator 接⼝部署在数据结构的Symbol.iterator 属性，或者说，⼀个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable）。Symbol.iterator 属性本身是⼀个函数，就是当前数据结构默认的遍历器⽣成函数。执⾏这个函数，就会返回⼀个遍历器。⾄于属性名Symbol.iterator，它是⼀个表达式，返回Symbol 对象的iterator 属性，这是⼀个预定义好的、类型为Symbol 的特殊值，所以要放在⽅括号内（参⻅Symbol ⼀章）。

ES6 的有些数据结构原⽣具备Iterator 接⼝（⽐如数组），即不⽤任何处理，就可以被for...of 循环遍历。原因在于，这些数据结构原⽣部署了Symbol.iterator 属性（详⻅下⽂），另外⼀些数据结构没有（⽐如对象）。凡是部署了Symbol.iterator 属性的数据结构，就称为部署了遍历器接⼝。调⽤这个接⼝，就会返回⼀个遍历器对象。

原⽣具备Iterator 接⼝的数据结构如下。
• Array
• Map
• Set
• String
• TypedArray
• 函数的arguments 对象
• NodeList 对象

对象（Object）之所以没有默认部署Iterator 接⼝，是因为对象的哪个属性先遍历，哪个属性后遍历是不确定的，需要开发者⼿动指定。本质上，遍历器是⼀种线性处理，对于任何⾮线性的数据结构，部署遍历器接⼝，就等于部署⼀种线性转换。不过，严格地说，对象部署遍历器接⼝并不是很必要，因为这时对象实际上被当作Map 结构使⽤，ES5 没有Map 结构，⽽ES6 原⽣提供了。

⼀个对象如果要具备可被for...of 循环调⽤的Iterator 接⼝，就必须在Symbol.iterator 的属性上部署遍历器⽣成⽅法（原型链上的对象具有该⽅法也可）。

对于类似数组的对象（存在数值键名和length 属性），部署Iterator 接⼝，有⼀个简便⽅法，就是Symbol.iterator ⽅法直接引⽤数组的Iterator 接⼝。

NodeList 对象是类似数组的对象，本来就具有遍历接⼝，可以直接遍历。

###### 调⽤Iterator 接⼝的场合

有⼀些场合会默认调⽤Iterator 接⼝（即Symbol.iterator ⽅法），除了下⽂会介绍的for...of 循环，还有⼏个别的场合。

（1）解构赋值

（2）扩展运算符

（3）yield *

（4）其他场合

由于数组的遍历会调⽤遍历器接⼝，所以任何接受数组作为参数的场合，其实都调⽤了遍历器接⼝。下⾯是⼀些例⼦。
• for…of
• Array.from()
• Map(), Set(), WeakMap(), WeakSet()（⽐如new Map([['a',1],['b',2]])）
• Promise.all()
• Promise.race()

###### 字符串的Iterator 接⼝

字符串是⼀个类似数组的对象，也原⽣具有Iterator 接⼝。

###### 遍历器对象的return()，throw()

遍历器对象除了具有next ⽅法，还可以具有return ⽅法和throw ⽅法。如果你⾃⼰写遍历器对象⽣成函数，那么next ⽅法是必须部署的，return ⽅法和throw ⽅法是否部署是可选的。
return ⽅法的使⽤场合是，如果for...of 循环提前退出（通常是因为出错，或者有break 语句或continue 语句），就会调⽤return ⽅法。如果⼀个对象在完成遍历前，需要清理或释放资源，就可以部署return ⽅法。

注意，return ⽅法必须返回⼀个对象，这是Generator 规格决定的。
throw ⽅法主要是配合Generator 函数使⽤，⼀般的遍历器对象⽤不到这个⽅法。

###### for…of 循环

ES6 借鉴C++、Java、C# 和Python 语⾔，引⼊了for...of 循环，作为遍历所有数据结构的统⼀的⽅法。
⼀个数据结构只要部署了Symbol.iterator 属性，就被视为具有iterator 接⼝，就可以⽤for...of 循环遍历它的成员。也就是说，for...of 循环内部调⽤的是数据结构的Symbol.iterator ⽅法。
for...of 循环可以使⽤的范围包括数组、Set 和Map 结构、某些类似数组的对象（⽐如arguments 对象、DOM NodeList 对象）、后⽂的Generator 对象，以及字符串。

数组

for...of 循环可以代替数组实例的forEach ⽅法。

JavaScript 原有的for...in 循环，只能获得对象的键名，不能直接获取键值。ES6 提供for...of 循环，允许遍历获得键值。

for...in 循环读取键名，for...of 循环读取键值。如果要通过for...of 循环，获取数组的索引，可以借助数组实例
的entries ⽅法和keys ⽅法，参⻅《数组的扩展》章节。
for...of 循环调⽤遍历器接⼝，数组的遍历器接⼝只返回具有数字索引的属性。这⼀点跟for...in 循环也不⼀样。

Set 和Map 结构

Set 和Map 结构也原⽣具有Iterator 接⼝，可以直接使⽤for...of 循环。

遍历Set 结构和Map 结构。值得注意的地⽅有两个，⾸先，遍历的顺序是按照各个成员被添加进数据结构的顺序。其次，Set 结构遍历时，返回的是⼀个值，⽽Map 结构遍历时，返回的是⼀个数组，该数组的两个成员分别为当前Map 成员的键名和键值。

计算⽣成的数据结构

有些数据结构是在现有数据结构的基础上，计算⽣成的。⽐如，ES6 的数组、Set、Map 都部署了以下三个⽅法，调⽤后都返回遍历器对象。
• entries() 返回⼀个遍历器对象，⽤来遍历[键名, 键值] 组成的数组。对于数组，键名就是索引值；对于Set，键名与键值相同。
Map 结构的Iterator 接⼝，默认就是调⽤entries ⽅法。
• keys() 返回⼀个遍历器对象，⽤来遍历所有的键名。
• values() 返回⼀个遍历器对象，⽤来遍历所有的键值。
这三个⽅法调⽤后⽣成的遍历器对象，所遍历的都是计算⽣成的数据结构。

类似数组的对象

类似数组的对象包括好⼏类。下⾯是for...of 循环⽤于字符串、DOM NodeList 对象、arguments 对象的例⼦。

对象

对于普通的对象，for...of 结构不能直接使⽤，会报错，必须部署了Iterator 接⼝后才能使⽤。但是，这样情况下，for...in 循环依然可以⽤来遍历键名。

与其他遍历语法的比较

以数组为例，JavaScript 提供多种遍历语法。最原始的写法就是for 循环。

这种写法⽐较麻烦，因此数组提供内置的forEach ⽅法。

这种写法的问题在于，⽆法中途跳出forEach 循环，break 命令或return 命令都不能奏效。

for...in 循环可以遍历数组的键名。

for...in 循环有⼏个缺点。
• 数组的键名是数字，但是for...in 循环是以字符串作为键名“0”、“1”、“2” 等等。
• for...in 循环不仅遍历数字键名，还会遍历⼿动添加的其他键，甚⾄包括原型链上的键。
• 某些情况下，for...in 循环会以任意顺序遍历键名。
总之，for...in 循环主要是为遍历对象⽽设计的，不适⽤于遍历数组。
for...of 循环相⽐上⾯⼏种做法，有⼀些显著的优点。

• 有着同for...in ⼀样的简洁语法，但是没有for...in 那些缺点。
• 不同于forEach ⽅法，它可以与break、continue 和return 配合使⽤。
• 提供了遍历所有数据结构的统⼀操作接⼝。

##### Generator函数的语法

###### 简介

Generator 函数有多种理解⻆度。从语法上，⾸先可以把它理解成，Generator 函数是⼀个状态机，封装了多个内部状态。
执⾏Generator 函数会返回⼀个遍历器对象，也就是说，Generator 函数除了状态机，还是⼀个遍历器对象⽣成函数。返回的遍历器对象，可以依次遍历Generator 函数内部的每⼀个状态。
形式上，Generator 函数是⼀个普通函数，但是有两个特征。⼀是，function 关键字与函数名之间有⼀个星号；⼆是，函数体内部使⽤yield 表达式，定义不同的内部状态（yield 在英语⾥的意思就是“产出”）。

```
function* helloWorldGenerator() {
	yield 'hello';
	yield 'world';
	return 'ending';
}
var hw = helloWorldGenerator();
```

上⾯代码定义了⼀个Generator 函数helloWorldGenerator，它内部有两个yield 表达式（hello 和world），即该函数有三个状态：hello，world 和return 语句（结束执⾏）。
然后，Generator 函数的调⽤⽅法与普通函数⼀样，也是在函数名后⾯加上⼀对圆括号。不同的是，调⽤Generator 函数后，该函数并不执⾏，返回的也不是函数运⾏结果，⽽是⼀个指向内部状态的指针对象，也就是上⼀章介绍的遍历器对象（Iterator Object）。
下⼀步，必须调⽤遍历器对象的next ⽅法，使得指针移向下⼀个状态。也就是说，每次调⽤next ⽅法，内部指针就从函数头部或上⼀次停下来的地⽅开始执⾏，直到遇到下⼀个yield 表达式（或return 语句）为⽌。换⾔之，Generator 函数是分段执⾏的，yield 表达式是暂停执⾏的标记，⽽next ⽅法可以恢复执⾏。

```
hw.next()
// { value: 'hello', done: false }
hw.next()
// { value: 'world', done: false }
hw.next()
// { value: 'ending', done: true }
hw.next()
// { value: undefined, done: true }
上⾯代码⼀共调⽤了四次next ⽅法。
```

第⼀次调⽤，Generator 函数开始执⾏，直到遇到第⼀个yield 表达式为⽌。next ⽅法返回⼀个对象，它的value 属性就是当前yield 表达式的值hello，done 属性的值false，表示遍历还没有结束。
第⼆次调⽤，Generator 函数从上次yield 表达式停下的地⽅，⼀直执⾏到下⼀个yield 表达式。next ⽅法返回的对象的value属性就是当前yield 表达式的值world，done 属性的值false，表示遍历还没有结束。
第三次调⽤，Generator 函数从上次yield 表达式停下的地⽅，⼀直执⾏到return 语句（如果没有return 语句，就执⾏到函数结束）。next ⽅法返回的对象的value 属性，就是紧跟在return 语句后⾯的表达式的值（如果没有return 语句，则value 属性的值为undefined），done 属性的值true，表示遍历已经结束。
第四次调⽤，此时Generator 函数已经运⾏完毕，next ⽅法返回对象的value 属性为undefined，done 属性为true。以后再调⽤next ⽅法，返回的都是这个值。

总结⼀下，调⽤Generator 函数，返回⼀个遍历器对象，代表Generator 函数的内部指针。以后，每次调⽤遍历器对象的next ⽅法，就会返回⼀个有着value 和done 两个属性的对象。value 属性表示当前的内部状态的值，是yield 表达式后⾯那个表达式的值；done属性是⼀个布尔值，表示是否遍历结束。

ES6 没有规定，function 关键字与函数名之间的星号，写在哪个位置。这导致下⾯的写法都能通过。
function * foo(x, y) { ··· }
function *foo(x, y) { ··· }
function* foo(x, y) { ··· }
function*foo(x, y) { ··· }
由于Generator 函数仍然是普通函数，所以⼀般的写法是上⾯的第三种，即星号紧跟在function 关键字后⾯。本书也采⽤这种写法。

###### yield 表达式

由于Generator 函数返回的遍历器对象，只有调⽤next ⽅法才会遍历下⼀个内部状态，所以其实提供了⼀种可以暂停执⾏的函数。yield表达式就是暂停标志。
遍历器对象的next ⽅法的运⾏逻辑如下。
（1）遇到yield 表达式，就暂停执⾏后⾯的操作，并将紧跟在yield 后⾯的那个表达式的值，作为返回的对象的value 属性值。
（2）下⼀次调⽤next ⽅法时，再继续往下执⾏，直到遇到下⼀个yield 表达式。
（3）如果没有再遇到新的yield 表达式，就⼀直运⾏到函数结束，直到return 语句为⽌，并将return 语句后⾯的表达式的值，作为返回的对象的value 属性值。
（4）如果该函数没有return 语句，则返回的对象的value 属性值为undefined。
需要注意的是，yield 表达式后⾯的表达式，只有当调⽤next ⽅法、内部指针指向该语句时才会执⾏，因此等于为JavaScript 提供了⼿动的“惰性求值”（Lazy Evaluation）的语法功能。

```
function* gen() {
yield 123 + 456;
}
```

上⾯代码中，yield 后⾯的表达式123 + 456，不会⽴即求值，只会在next ⽅法将指针移到这⼀句时，才会求值。
yield 表达式与return 语句既有相似之处，也有区别。相似之处在于，都能返回紧跟在语句后⾯的那个表达式的值。区别在于每次遇到yield，函数暂停执⾏，下⼀次再从该位置继续向后执⾏，⽽return 语句不具备位置记忆的功能。⼀个函数⾥⾯，只能执⾏⼀次（或者说⼀个）return 语句，但是可以执⾏多次（或者说多个）yield 表达式。正常函数只能返回⼀个值，因为只能执⾏⼀次return；Generator 函数可以返回⼀系列的值，因为可以有任意多个yield。从另⼀个⻆度看，也可以说Generator ⽣成了⼀系列的值，这也就是它的名称的来历（英语中，generator 这个词是“⽣成器” 的意思）。

Generator 函数可以不⽤yield 表达式，这时就变成了⼀个单纯的暂缓执⾏函数。

```
function* f() {
	console.log(' 执⾏了！')
}
var generator = f();
setTimeout(function () {
	generator.next()
}, 2000);
```

上⾯代码中，函数f 如果是普通函数，在为变量generator 赋值时就会执⾏。但是，函数f 是⼀个Generator 函数，就变成只有调⽤next ⽅法时，函数f 才会执⾏。

另外需要注意，yield 表达式只能⽤在Generator 函数⾥⾯，⽤在其他地⽅都会报错。

另外，yield 表达式如果⽤在另⼀个表达式之中，必须放在圆括号⾥⾯。

```
function* demo() {
	console.log('Hello' + yield); // SyntaxError
	console.log('Hello' + yield 123); // SyntaxError
	console.log('Hello' + (yield)); // OK
	console.log('Hello' + (yield 123)); // OK
}
```

yield 表达式⽤作函数参数或放在赋值表达式的右边，可以不加括号。

```
function* demo() {
	foo(yield 'a', yield 'b'); // OK
	let input = yield; // OK
}
```

###### 与Iterator 接⼝的关系

由于Generator 函数就是遍历器⽣成函数，因此可以把Generator 赋值给对象的Symbol.iterator 属性，从⽽使得该对象具有Iterator 接⼝。

Generator 函数执⾏后，返回⼀个遍历器对象。该对象本身也具有Symbol.iterator 属性，执⾏后返回⾃身。

###### next ⽅法的参数

yield 表达式本身没有返回值，或者说总是返回undefined。next ⽅法可以带⼀个参数，该参数就会被当作上⼀个yield 表达式的返回值。

```
function* f() {
	for(var i = 0; true; i++) {
		var reset = yield i;
		if(reset) { i = -1; }
	}
}
var g = f();
g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next(true) // { value: 0, done: false }
```

上⾯代码先定义了⼀个可以⽆限运⾏的Generator 函数f，如果next ⽅法没有参数，每次运⾏到yield 表达式，变量reset 的值总是undefined。当next ⽅法带⼀个参数true 时，变量reset 就被重置为这个参数（即true），因此i 会等于-1，下⼀轮循环就会从-1 开始递增。
这个功能有很重要的语法意义。Generator 函数从暂停状态到恢复运⾏，它的上下⽂状态（context）是不变的。通过next ⽅法的参数，就有办法在Generator 函数开始运⾏之后，继续向函数体内部注⼊值。也就是说，可以在Generator 函数运⾏的不同阶段，从外部向内部注⼊不同的值，从⽽调整函数⾏为。

注意，由于next ⽅法的参数表示上⼀个yield 表达式的返回值，所以在第⼀次使⽤next ⽅法时，传递参数是⽆效的。V8 引擎直接忽略第⼀次使⽤next ⽅法时的参数，只有从第⼆次使⽤next ⽅法开始，参数才是有效的。从语义上讲，第⼀个next ⽅法⽤来启动遍历器对象，所以不⽤带有参数。

###### for…of 循环

for...of 循环可以⾃动遍历Generator 函数时⽣成的Iterator 对象，且此时不再需要调⽤next ⽅法。

```
function *foo() {
	yield 1;
	yield 2;
	yield 3;
	yield 4;
	yield 5;
	return 6;
}
for (let v of foo()) {
	console.log(v);
}
// 1 2 3 4 5
```

上⾯代码使⽤for...of 循环，依次显示5 个yield 表达式的值。这⾥需要注意，⼀旦next ⽅法的返回对象的done 属性为true，for...of 循环就会中⽌，且不包含该返回对象，所以上⾯代码的return 语句返回的6，不包括在for...of 循环之中。

利⽤for...of 循环，可以写出遍历任意对象（object）的⽅法。原⽣的JavaScript 对象没有遍历接⼝，⽆法使⽤for...of 循环，通过Generator 函数为它加上这个接⼝，就可以⽤了。

除了for...of 循环以外，扩展运算符（...）、解构赋值和Array.from ⽅法内部调⽤的，都是遍历器接⼝。这意味着，它们都可以将Generator 函数返回的Iterator 对象，作为参数。

```
function* numbers () {
	yield 1
	yield 2
	return 3
	yield 4
}
// 扩展运算符
[...numbers()] // [1, 2]
// Array.from ⽅法
Array.from(numbers()) // [1, 2]
// 解构赋值
let [x, y] = numbers();
x // 1
y // 2
// for...of 循环
for (let n of numbers()) {
console.log(n)
}
// 1
// 2
```

###### Generator.prototype.throw()

Generator 函数返回的遍历器对象，都有⼀个throw ⽅法，可以在函数体外抛出错误，然后在Generator 函数体内捕获。

###### Generator.prototype.return()

Generator 函数返回的遍历器对象，还有⼀个return ⽅法，可以返回给定的值，并且终结遍历Generator 函数。

```
function* gen() {
	yield 1;
	yield 2;
	yield 3;
}
var g = gen();
g.next() // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next() // { value: undefined, done: true }
```

上⾯代码中，遍历器对象g 调⽤return ⽅法后，返回值的value 属性就是return ⽅法的参数foo。并且，Generator 函数的遍历就终⽌了，返回值的done 属性为true，以后再调⽤next ⽅法，done 属性总是返回true。
如果return ⽅法调⽤时，不提供参数，则返回值的value 属性为undefined。

```
function* gen() {
	yield 1;
	yield 2;
	yield 3;
}
var g = gen();
g.next() // { value: 1, done: false }
g.return() // { value: undefined, done: true }
```

如果Generator 函数内部有try...finally 代码块，那么return ⽅法会推迟到finally 代码块执⾏完再执⾏。

```
function* numbers () {
yield 1;
try {
	yield 2;
	yield 3;
} finally {
	yield 4;
	yield 5;
}
yield 6;
}
var g = numbers();
g.next() // { value: 1, done: false }
g.next() // { value: 2, done: false }
g.return(7) // { value: 4, done: false }
g.next() // { value: 5, done: false }
g.next() // { value: 7, done: true }
```

⾯代码中，调⽤return ⽅法后，就开始执⾏finally 代码块，然后等到finally 代码块执⾏完，再执⾏return ⽅法。

###### next()、throw()、return() 的共同点

next()、throw()、return() 这三个⽅法本质上是同⼀件事，可以放在⼀起理解。它们的作⽤都是让Generator
函数恢复执⾏，并且使⽤不同的语句替换yield 表达式。
next() 是将yield 表达式替换成⼀个值。

throw() 是将yield 表达式替换成⼀个throw 语句。

return() 是将yield 表达式替换成⼀个return 语句。

###### yield* 表达式

如果在Generator 函数内部，调⽤另⼀个Generator 函数，默认情况下是没有效果的。

```
function* foo() {
	yield 'a';
	yield 'b';
}
function* bar() {
	yield 'x';
	foo();
	yield 'y';
}
for (let v of bar()){
	console.log(v);
}
// "x"
// "y"
```

上⾯代码中，foo 和bar 都是Generator 函数，在bar ⾥⾯调⽤foo，是不会有效果的。
这个就需要⽤到yield* 表达式，⽤来在⼀个Generator 函数⾥⾯执⾏另⼀个Generator 函数。

```
function* bar() {
	yield 'x';
	yield* foo();
	yield 'y';
}
// 等同于
function* bar() {
	yield 'x';
	yield 'a';
	yield 'b';
	yield 'y';
}
// 等同于
function* bar() {
	yield 'x';
	for (let v of foo()) {
		yield v;
	}
	yield 'y';
}
for (let v of bar()){
	console.log(v);
}
// "x"
// "a"
// "b"
// "y"
```

从语法⻆度看，如果yield 表达式后⾯跟的是⼀个遍历器对象，需要在yield 表达式后⾯加上星号，表明它返回的是⼀个遍历器对象。这被称为yield* 表达式。

yield* 后⾯的Generator 函数（没有return 语句时），等同于在Generator 函数内部，部署⼀个for...of 循环。

如果yield* 后⾯跟着⼀个数组，由于数组原⽣⽀持遍历器，因此就会遍历数组成员。

```
function* gen(){
	yield* ["a", "b", "c"];
}
gen().next() // { value:"a", done:false }
```

上⾯代码中，yield 命令后⾯如果不加星号，返回的是整个数组，加了星号就表示返回的是数组的遍历器对象。
实际上，任何数据结构只要有Iterator 接⼝，就可以被yield* 遍历。

###### Generator 函数的this

Generator 函数总是返回⼀个遍历器，ES6 规定这个遍历器是Generator 函数的实例，也继承了Generator 函数的prototype 对象上的⽅法。

Generator 函数也不能跟new 命令⼀起⽤，会报错。

###### Generator 与状态机

Generator 是实现状态机的最佳结构。

###### Generator 与协程

协程（coroutine）是⼀种程序运⾏的⽅式，可以理解成“协作的线程” 或“协作的函数”。协程既可以⽤单线程实现，也可以⽤多线程实现。前者是⼀种特殊的⼦例程，后者是⼀种特殊的线程。

Generator 函数是ES6 对协程的实现，但属于不完全实现。Generator 函数被称为“半协程”（semi-coroutine），意思是只有Generator 函数的调⽤者，才能将程序的执⾏权还给Generator 函数。如果是完全执⾏的协程，任何函数都可以让暂停的协程继续执⾏。
如果将Generator 函数当作协程，完全可以将多个需要互相协作的任务写成Generator 函数，它们之间使⽤yield 表示式交换控制权。

###### 应用

Generator 可以暂停函数执⾏，返回任意表达式的值。这种特点使得Generator 有多种应⽤场景。

（1）异步操作的同步化表达

Generator 函数的暂停执⾏的效果，意味着可以把异步操作写在yield 表达式⾥⾯，等到调⽤next ⽅法时再往后执⾏。这实际上等同于不需要写回调函数了，因为异步操作的后续操作可以放在yield 表达式下⾯，反正要等到调⽤next ⽅法时再执⾏。所以，Generator 函数的⼀个重要实际意义就是⽤来处理异步操作，改写回调函数。

Ajax 是典型的异步操作，通过Generator 函数部署Ajax 操作，可以⽤同步的⽅式表达。

```
function* main() {
	var result = yield request("http://some.url");
	var resp = JSON.parse(result);
	console.log(resp.value);
}
```

（2）控制流管理

（3）部署Iterator 接⼝

利⽤Generator 函数，可以在任意对象上部署Iterator 接⼝。

（4）作为数据结构

Generator 可以看作是数据结构，更确切地说，可以看作是⼀个数组结构，因为Generator 函数可以返回⼀系列的值，这意味着它可以对任意表达式，提供类似数组的接⼝。

###### Generator函数的异步应用

###### 1.传统方法

ES6 诞⽣以前，异步编程的⽅法，⼤概有下⾯四种。
• 回调函数
• 事件监听
• 发布/订阅
• Promise 对象
Generator 函数将JavaScript 异步编程带⼊了⼀个全新的阶段。

###### 2.基本概念

所谓“异步”，简单说就是⼀个任务不是连续完成的，可以理解成该任务被⼈为分成两段，先执⾏第⼀段，然后转⽽执⾏其他任务，等做好了准备，再回过头执⾏第⼆段。
⽐如，有⼀个任务是读取⽂件进⾏处理，任务的第⼀段是向操作系统发出请求，要求读取⽂件。然后，程序执⾏其他任务，等到操作系统返回⽂件，再接着执⾏任务的第⼆段（处理⽂件）。这种不连续的执⾏，就叫做异步。
相应地，连续的执⾏就叫做同步。由于是连续执⾏，不能插⼊其他任务，所以操作系统从硬盘读取⽂件的这段时间，程序只能⼲等着。

###### 回调函数

JavaScript 语⾔对异步编程的实现，就是回调函数。所谓回调函数，就是把任务的第⼆段单独写在⼀个函数⾥⾯，等到重新执⾏这个任务的时候，就直接调⽤这个函数。回调函数的英语名字callback，直译过来就是“重新调⽤”。

###### Promise

回调函数本身并没有问题，它的问题出现在多个回调函数嵌套。

因为多个异步操作形成了强耦合，只要有⼀个操作需要修改，它的上层回调函数和下层回调函数，可能都要跟着修改。这种情况就称为“回调函数地狱”（callbackhell）。
Promise 对象就是为了解决这个问题⽽提出的。它不是新的语法功能，⽽是⼀种新的写法，允许将回调函数的嵌套，改成链式调⽤。

Promise 提供then ⽅法加载回调函数，catch ⽅法捕捉执⾏过程中抛出的错误。
可以看到，Promise 的写法只是回调函数的改进，使⽤then ⽅法以后，异步任务的两段执⾏看得更清楚了，除此以外，并⽆新意。

Promise 的最⼤问题是代码冗余，原来的任务被Promise 包装了⼀下，不管什么操作，⼀眼看去都是⼀堆then，原来的语义变得很不清楚。
那么，有没有更好的写法呢？

###### Generator函数

Generator 函数是协程在ES6 的实现，最⼤特点就是可以交出函数的执⾏权（即暂停执⾏）。
整个Generator 函数就是⼀个封装的异步任务，或者说是异步任务的容器。异步操作需要暂停的地⽅，都⽤yield 语句注明。Generator函数的执⾏⽅法如下。

```
function* gen(x) {
	var y = yield x + 2;
	return y;
}
var g = gen(1);
g.next() // { value: 3, done: false }
g.next() // { value: undefined, done: true }
```

上⾯代码中，调⽤Generator 函数，会返回⼀个内部指针（即遍历器）g。这是Generator 函数不同于普通函数的另⼀个地⽅，即执⾏它不会返回结果，返回的是指针对象。调⽤指针g 的next ⽅法，会移动内部指针（即执⾏异步任务的第⼀段），指向第⼀个遇到的yield 语句，上例是执⾏到x + 2 为⽌。
换⾔之，next ⽅法的作⽤是分阶段执⾏Generator 函数。每次调⽤next ⽅法，会返回⼀个对象，表示当前阶段的信息（value 属性和done 属性）。value 属性是yield 语句后⾯表达式的值，表示当前阶段的值；done 属性是⼀个布尔值，表示Generator 函数是否执⾏完毕，即是否还有下⼀个阶段。

###### Generator 函数的数据交换和错误处理

Generator 函数可以暂停执⾏和恢复执⾏，这是它能封装异步任务的根本原因。除此之外，它还有两个特性，使它可以作为异步编程的完整解决⽅案：函数体内外的数据交换和错误处理机制。
next 返回值的value 属性，是Generator 函数向外输出数据；next ⽅法还可以接受参数，向Generator 函数体内输⼊数据。

Generator 函数内部还可以部署错误处理代码，捕获函数体外抛出的错误。

```
function* gen(x){
	try {
		var y = yield x + 2;
	} catch (e){
		console.log(e);
	}
	return y;
}
var g = gen(1);
g.next();
g.throw(' 出错了');
// 出错了
```

上⾯代码的最后⼀⾏，Generator 函数体外，使⽤指针对象的throw ⽅法抛出的错误，可以被函数体内的try...catch 代码块捕获。这意味着，出错的代码与处理错误的代码，实现了时间和空间上的分离，这对于异步编程⽆疑是很重要的。

###### Thunk函数

Thunk 函数是⾃动执⾏Generator 函数的⼀种⽅法。

参数的求值策略

⼀种意⻅是“传值调⽤”（call by value），即在进⼊函数体之前，就计算x + 5 的值（等于6），再将这个值传⼊函数f。C 语⾔就采⽤这种策略。
f(x + 5)
// 传值调⽤时，等同于
f(6)

另⼀种意⻅是“传名调⽤”（call by name），即直接将表达式x + 5 传⼊函数体，只在⽤到它的时候求值。Haskell 语⾔采⽤这种策略。

f(x + 5)
// 传名调⽤时，等同于
(x + 5) * 2
传值调⽤和传名调⽤，哪⼀种⽐较好？
回答是各有利弊。传值调⽤⽐较简单，但是对参数求值的时候，实际上还没⽤到这个参数，有可能造成性能损失。

###### Thunk 函数的含义

编译器的“传名调⽤” 实现，往往是将参数放到⼀个临时函数之中，再将这个临时函数传⼊函数体。这个临时函数就叫做Thunk 函数。

###### JavaScript 语⾔的Thunk 函数

JavaScript 语⾔是传值调⽤，它的Thunk 函数含义有所不同。在JavaScript 语⾔中，Thunk 函数替换的不是表达式，⽽是多参数函数，将其替换成⼀个只接受回调函数作为参数的单参数函数。

###### Thunkify 模块

⽣产环境的转换器，建议使⽤Thunkify 模块。

###### Generator 函数的流程管理

你可能会问，Thunk 函数有什么⽤？回答是以前确实没什么⽤，但是ES6 有了Generator 函数，Thunk 函数现在可以⽤于Generator函数的⾃动流程管理。
Generator 函数可以⾃动执⾏。

###### Thunk 函数的⾃动流程管理

Thunk 函数真正的威⼒，在于可以⾃动执⾏Generator 函数。

###### co 模块

co 模块是著名程序员TJ Holowaychuk 于2013 年6 ⽉发布的⼀个⼩⼯具，⽤于Generator 函数的⾃动执⾏。

为什么co 可以⾃动执⾏Generator 函数？
前⾯说过，Generator 就是⼀个异步操作的容器。它的⾃动执⾏需要⼀种机制，当异步操作有了结果，能够⾃动交回执⾏权。
两种⽅法可以做到这⼀点。
（1）回调函数。将异步操作包装成Thunk 函数，在回调函数⾥⾯交回执⾏权。
（2）Promise 对象。将异步操作包装成Promise 对象，⽤then ⽅法交回执⾏权。
co 模块其实就是将两种⾃动执⾏器（Thunk 函数和Promise 对象），包装成⼀个模块。使⽤co 的前提条件是，Generator 函数的yield命令后⾯，只能是Thunk 函数或Promise 对象。如果数组或对象的成员，全部都是Promise 对象，也可以使⽤co，

##### async函数

###### 含义

ES2017 标准引⼊了async 函数，使得异步操作变得更加⽅便。
async 函数是什么？⼀句话，它就是Generator 函数的语法糖。

⼀⽐较就会发现，async 函数就是将Generator 函数的星号（*）替换成async，将yield 替换成await，仅此⽽已。

async 函数对Generator 函数的改进，体现在以下四点。
（1）内置执⾏器。
Generator 函数的执⾏必须靠执⾏器，所以才有了co 模块，⽽async 函数⾃带执⾏器。也就是说，async 函数的执⾏，与普通函数⼀模⼀样，只要⼀⾏。
（2）更好的语义。
async 和await，⽐起星号和yield，语义更清楚了。async 表示函数⾥有异步操作，await 表示紧跟在后⾯的表达式需要等待结果。
（3）更⼴的适⽤性。
co 模块约定，yield 命令后⾯只能是Thunk 函数或Promise 对象，⽽async 函数的await 命令后⾯，可以是Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）。
（4）返回值是Promise
async 函数的返回值是Promise 对象，这⽐Generator 函数的返回值是Iterator 对象⽅便多了。你可以⽤then ⽅法指定下⼀步的操作。
进⼀步说，async 函数完全可以看作多个异步操作，包装成的⼀个Promise 对象，⽽await 命令就是内部then 命令的语法糖。

###### 基本用法

async 函数返回⼀个Promise 对象，可以使⽤then ⽅法添加回调函数。当函数执⾏的时候，⼀旦遇到await 就会先返回，等到异步操作完成，再接着执⾏函数体内后⾯的语句。

async 函数有多种使⽤形式。

// 函数声明
async function foo() {}
// 函数表达式
const foo = async function () {};
// 对象的⽅法
let obj = { async foo() {} };
obj.foo().then(...)
// Class 的⽅法
class Storage {
	constructor() {
		this.cachePromise = caches.open('avatars');
	}

}

async getAvatar(name) {
	const cache = await this.cachePromise;
	return cache.match(`/avatars/${name}.jpg`);
}
const storage = new Storage();
storage.getAvatar('jake').then(…);
// 箭头函数
const foo = async () => {};

###### 语法

async 函数的语法规则总体上⽐较简单，难点是错误处理机制。

###### 返回Promise 对象

async 函数返回⼀个Promise 对象。
async 函数内部return 语句返回的值，会成为then ⽅法回调函数的参数。

async 函数内部抛出错误，会导致返回的Promise 对象变为reject 状态。抛出的错误对象会被catch ⽅法回调函数接收到。

###### Promise 对象的状态变化

async 函数返回的Promise 对象，必须等到内部所有await 命令后⾯的Promise 对象执⾏完，才会发⽣状态改变，除⾮遇到return语句或者抛出错误。也就是说，只有async 函数内部的异步操作执⾏完，才会执⾏then ⽅法指定的回调函数。

###### await 命令

正常情况下，await 命令后⾯是⼀个Promise 对象。如果不是，会被转成⼀个⽴即resolve 的Promise 对象。

await 命令后⾯的Promise 对象如果变为reject 状态，则reject 的参数会被catch ⽅法的回调函数接收到。

只要⼀个await 语句后⾯的Promise 变为reject，那么整个async 函数都会中断执⾏。

```
async function f() {
await Promise.reject(' 出错了');
await Promise.resolve('hello world'); // 不会执⾏
}
上⾯代码中，第⼆个await 语句是不会执⾏的，因为第⼀个await 语句状态变成了reject。
```

有时，我们希望即使前⼀个异步操作失败，也不要中断后⾯的异步操作。这时可以将第⼀个await 放在try...catch 结构⾥⾯，这样不管这个异步操作是否成功，第⼆个await 都会执⾏。

```
async function f() {
	try {
		await Promise.reject(' 出错了');
	} catch(e) {
	}
	return await Promise.resolve('hello world');
}
f()
.then(v => console.log(v))
// hello world
```

另⼀种⽅法是await 后⾯的Promise 对象再跟⼀个catch ⽅法，处理前⾯可能出现的错误。

```
async function f() {
	await Promise.reject(' 出错了')
		.catch(e => console.log(e));
	return await Promise.resolve('hello world');
}
f()
.then(v => console.log(v))
// 出错了
// hello world
```

###### 错误处理

如果await 后⾯的异步操作出错，那么等同于async 函数返回的Promise 对象被reject。

防⽌出错的⽅法，也是将其放在try...catch 代码块之中。

如果有多个await 命令，可以统⼀放在try...catch 结构中。

###### 使用注意点

第⼀点，前⾯已经说过，await 命令后⾯的Promise 对象，运⾏结果可能是rejected，所以最好把await 命令放在try...catch代码块中。

第⼆点，多个await 命令后⾯的异步操作，如果不存在继发关系，最好让它们同时触发。

第三点，await 命令只能⽤在async 函数之中，如果⽤在普通函数，就会报错。

###### async函数的实现原理

async 函数的实现原理，就是将Generator 函数和⾃动执⾏器，包装在⼀个函数⾥。

###### 异步遍历器

《遍历器》⼀章说过，Iterator 接⼝是⼀种数据遍历的协议，只要调⽤遍历器对象的next ⽅法，就会得到⼀个对象，表示当前遍历指针所在的那个位置的信息。next ⽅法返回的对象的结构是{value, done}，其中value 表示当前的数据的值，done 是⼀个布尔值，表示遍历是否结束。
这⾥隐含着⼀个规定，next ⽅法必须是同步的，只要调⽤就必须⽴刻返回值。也就是说，⼀旦执⾏next ⽅法，就必须同步地得到value 和done 这两个属性。如果遍历指针正好指向同步操作，当然没有问题，但对于异步操作，就不太合适了。⽬前的解决⽅法是，Generator 函数⾥⾯的异步操作，返回⼀个Thunk 函数或者Promise 对象，即value 属性是⼀个Thunk 函数或者Promise 对象，等待以后返回真正的值，⽽done 属性则还是同步产⽣的。
⽬前，有⼀个提案，为异步操作提供原⽣的遍历器接⼝，即value 和done 这两个属性都是异步产⽣，这称为” 异步遍历器“（AsyncIterator）。

###### 异步遍历的接⼝

异步遍历器的最⼤的语法特点，就是调⽤遍历器的next ⽅法，返回的是⼀个Promise 对象。

我们知道， ⼀个对象的同步遍历器的接⼝， 部署在Symbol.iterator 属性上⾯。同样地， 对象的异步遍历器接⼝， 部署在Symbol.asyncIterator 属性上⾯。不管是什么样的对象，只要它的Symbol.asyncIterator 属性有值，就表示应该对它进⾏异步遍历。

###### for await…of

前⾯介绍过，for...of 循环⽤于遍历同步的Iterator 接⼝。新引⼊的for await...of 循环，则是⽤于遍历异步的Iterator接⼝。

for await...of 循环的⼀个⽤途，是部署了asyncIterable 操作的异步接⼝，可以直接放⼊这个循环。

如果next ⽅法返回的Promise 对象被reject，for await...of 就会报错，要⽤try...catch 捕捉。

注意，for await...of 循环也可以⽤于同步遍历器。

###### 异步Generator 函数

就像Generator 函数返回⼀个同步遍历器对象⼀样，异步Generator 函数的作⽤，是返回⼀个异步遍历器对象。
在语法上，异步Generator 函数就是async 函数与Generator 函数的结合。

```
async function* gen() {
	yield 'hello';
}
const genObj = gen();
genObj.next().then(x => console.log(x));
// { value: 'hello', done: false }
```

异步遍历器的设计⽬的之⼀，就是Generator 函数处理同步操作和异步操作时，能够使⽤同⼀套接⼝。

异步Generator 函数内部，能够同时使⽤await 和yield 命令。可以这样理解，await 命令⽤于将外部操作产⽣的值输⼊函数内部，yield 命令⽤于将函数内部的值输出。

异步Generator 函数可以与for await...of 循环结合起来使⽤。

异步Generator 函数的返回值是⼀个异步Iterator，即每次调⽤它的next ⽅法，会返回⼀个Promise 对象，也就是说，跟在yield命令后⾯的，应该是⼀个Promise 对象。

如果异步Generator 函数抛出错误，会被Promise 对象reject，然后抛出的错误被catch ⽅法捕获。

异步Generator 函数出现以后，JavaScript 就有了四种函数形式：普通函数、async 函数、Generator 函数和异步Generator 函数。请注意区分每种函数的不同之处。基本上，如果是⼀系列按照顺序执⾏的异步操作（⽐如读取⽂件，然后写⼊新内容，再存⼊硬盘），可以使⽤async 函数；如果是⼀系列产⽣相同数据结构的异步操作（⽐如⼀⾏⼀⾏读取⽂件），可以使⽤异步Generator 函数。
异步Generator 函数也可以通过next ⽅法的参数，接收外部传⼊的数据。

###### yield* 语句

yield* 语句也可以跟⼀个异步遍历器。

##### Class的基本语法

###### 简介

JavaScript 语⾔中，⽣成实例对象的传统⽅法是通过构造函数。



ES6 提供了更接近传统语⾔的写法，引⼊了Class（类）这个概念，作为对象的模板。通过class 关键字，可以定义类。
基本上，ES6 的class 可以看作只是⼀个语法糖，它的绝⼤部分功能，ES5 都可以做到，新的class 写法只是让对象原型的写法更加清晰更像⾯向对象编程的语法⽽已。

注意，定义“类” 的⽅法的时候，前⾯不需要加上function 这个关键字，直接把函数定义放进去了就可以了。另外，⽅法之间不需要逗号分隔，加了会报错。

使⽤的时候，也是直接对类使⽤new 命令，跟构造函数的⽤法完全⼀致。

构造函数的prototype 属性，在ES6 的“类” 上⾯继续存在。事实上，类的所有⽅法都定义在类的prototype 属性上⾯。

另外，类的内部所有定义的⽅法，都是不可枚举的（non-enumerable）。

在类的实例上⾯调⽤⽅法，其实就是调⽤原型上的⽅法。

由于类的⽅法都定义在prototype 对象上⾯，所以类的新⽅法可以添加在prototype 对象上⾯。Object.assign ⽅法可以很⽅便地⼀次向类添加多个⽅法。

类的属性名，可以采⽤表达式。

###### 严格模式

类和模块的内部，默认就是严格模式，所以不需要使⽤use strict 指定运⾏模式。只要你的代码写在类或模块之中，就只有严格模式可⽤。
考虑到未来所有的代码，其实都是运⾏在模块之中，所以ES6 实际上把整个语⾔升级到了严格模式。

###### constructor ⽅法

constructor ⽅法是类的默认⽅法，通过new 命令⽣成对象实例时，⾃动调⽤该⽅法。⼀个类必须有constructor ⽅法，如果没有显式定义，⼀个空的constructor ⽅法会被默认添加。

```
class Point {
}
// 等同于
class Point {
	constructor() {}
}
```

constructor ⽅法默认返回实例对象（即this），完全可以指定返回另外⼀个对象。

类必须使⽤new 调⽤，否则会报错。这是它跟普通构造函数的⼀个主要区别，后者不⽤new 也可以执⾏。

###### 类的实例对象

⽣成类的实例对象的写法，与ES5 完全⼀样，也是使⽤new 命令。前⾯说过，如果忘记加上new，像函数那样调⽤Class，将会报错。

与ES5 ⼀样，实例的属性除⾮显式定义在其本身（即定义在this 对象上），否则都是定义在原型上（即定义在class 上）。

与ES5 ⼀样，类的所有实例共享⼀个原型对象。

⽣产环境中，我们可以使⽤Object.getPrototypeOf⽅法来获取实例对象的原型，然后再来为原型添加⽅法/属性。

###### Class表达式

与函数一样，类也可以使用表达式的形式定义。

```
const MyClass = class Me {
	getClassName() {
		return Me.name;
	}
};
```

上⾯代码使⽤表达式定义了⼀个类。需要注意的是，这个类的名字是MyClass ⽽不是Me，Me 只在Class 的内部代码可⽤，指代当前类。

如果类的内部没⽤到的话，可以省略Me，也就是可以写成下⾯的形式。

```
const MyClass = class { /* ... */ };
```

采⽤Class 表达式，可以写出⽴即执⾏的Class。

```
let person = new class {
	constructor(name) {
		this.name = name;
	}
	sayName() {
		console.log(this.name);
	}
}(' 张三');
person.sayName(); // " 张三"
```

上⾯代码中，person 是⼀个⽴即执⾏的类的实例。

###### 不存在变量提升

类不存在变量提升（hoist），这一点与ES5完全不同。

```
new Foo(); // ReferenceError
class Foo {}
```

上⾯代码中，Foo 类使⽤在前，定义在后，这样会报错，因为ES6 不会把类的声明提升到代码头部。这种规定的原因与下⽂要提到的继承有关，必须保证⼦类在⽗类之后定义。

###### 私有方法

私有⽅法是常⻅需求，但ES6 不提供，只能通过变通⽅法模拟实现。
⼀种做法是在命名上加以区别。

```
class Widget {
	// 公有⽅法
	foo (baz) {
		this._bar(baz);
	}
	// 私有⽅法
	_bar(baz) {
		return this.snaf = baz;
	}
	// ...
}
```

上⾯代码中，_bar ⽅法前⾯的下划线，表示这是⼀个只限于内部使⽤的私有⽅法。但是，这种命名是不保险的，在类的外部，还是可以调⽤到这个⽅法。

另⼀种⽅法就是索性将私有⽅法移出模块，因为模块内部的所有⽅法都是对外可⻅的。

```
class Widget {
	foo (baz) {
		bar.call(this, baz);
	}
	// ...
}
function bar(baz) {
	return this.snaf = baz;
}
```

上⾯代码中，foo 是公有⽅法，内部调⽤了bar.call(this, baz)。这使得bar 实际上成为了当前模块的私有⽅法。

还有⼀种⽅法是利⽤Symbol 值的唯⼀性，将私有⽅法的名字命名为⼀个Symbol 值。

```
const bar = Symbol('bar');
const snaf = Symbol('snaf');
export default class myClass{
// 公有⽅法
foo(baz) {
	this[bar](baz);
}
// 私有⽅法
[bar](baz) {
	return this[snaf] = baz;
}
// ...
};
```

上⾯代码中，bar 和snaf 都是Symbol 值，导致第三⽅⽆法获取到它们，因此达到了私有⽅法和私有属性的效果。

###### 私有属性

与私有⽅法⼀样，ES6 不⽀持私有属性。⽬前，有⼀个提案，为class 加了私有属性。⽅法是在属性名之前，使⽤# 表示。

私有属性可以指定初始值，在构造函数执⾏时进⾏初始化。

之所以要引⼊⼀个新的前缀# 表示私有属性，⽽没有采⽤private 关键字，是因为JavaScript 是⼀⻔动态语⾔，使⽤独⽴的符号似乎是唯⼀的可靠⽅法，能够准确地区分⼀种属性是否为私有属性。另外，Ruby 语⾔使⽤@ 表示私有属性，ES6 没有⽤这个符号⽽使⽤#，是因为@已经被留给了Decorator。
该提案只规定了私有属性的写法。但是，很⾃然地，它也可以⽤来写私有⽅法。

###### this的指向

类的⽅法内部如果含有this，它默认指向类的实例。但是，必须⾮常⼩⼼，⼀旦单独使⽤该⽅法，很可能报错。

⼀个⽐较简单的解决⽅法是，在构造⽅法中绑定this，这样就不会找不到print ⽅法了。

另⼀种解决⽅法是使⽤箭头函数。

还有⼀种解决⽅法是使⽤Proxy，获取⽅法的时候，⾃动绑定this。

###### name属性

由于本质上，ES6 的类只是ES5 的构造函数的⼀层包装，所以函数的许多特性都被Class 继承，包括name 属性。

```
class Point {}
Point.name // "Point"
```

name 属性总是返回紧跟在class 关键字后⾯的类名。

###### Class 的取值函数（getter）和存值函数（setter）

与ES5 ⼀样，在“类” 的内部可以使⽤get 和set 关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取⾏为。

存值函数和取值函数是设置在属性的Descriptor 对象上的。

###### Class 的Generator ⽅法

如果某个⽅法之前加上星号（*），就表示该⽅法是⼀个Generator 函数。

###### Class的静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```
class Foo {
	static classMethod() {
		return 'hello';
	}
}
Foo.classMethod() // 'hello'
var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

上⾯代码中，Foo 类的classMethod ⽅法前有static 关键字， 表明该⽅法是⼀个静态⽅法， 可以直接在Foo 类上调⽤（Foo.classMethod()），⽽不是在Foo 类的实例上调⽤。如果在实例上调⽤静态⽅法，会抛出⼀个错误，表示不存在该⽅法。

注意，如果静态⽅法包含this 关键字，这个this 指的是类，⽽不是实例。

静态⽅法可以与⾮静态⽅法重名。

⽗类的静态⽅法，可以被⼦类继承。

静态⽅法也是可以从super 对象上调⽤的。

###### Class 的静态属性和实例属性

静态属性指的是Class 本身的属性，即Class.propName，⽽不是定义在实例对象（this）上的属性。

```
class Foo{}
Foo.prop = 1;
Foo.prop //1
```

上⾯的写法为Foo 类定义了⼀个静态属性prop。
⽬前，只有这种写法可⾏，因为ES6 明确规定，Class 内部只有静态⽅法，没有静态属性。

```
class Foo {
	// 写法⼀
	prop: 2
	// 写法⼆
	static prop: 2
}
Foo.prop // undefined
```

（1）类的实例属性

类的实例属性可以⽤等式，写⼊类的定义之中。

（2）类的静态属性

类的静态属性只要在上⾯的实例属性写法前⾯，加上static 关键字就可以了。

###### new.target 属性

new 是从构造函数⽣成实例对象的命令。ES6 为new 命令引⼊了⼀个new.target 属性，该属性⼀般⽤在构造函数之中，返回new 命令作⽤于的那个构造函数。如果构造函数不是通过new 命令调⽤的，new.target 会返回undefined，因此这个属性可以⽤来确定构造函数是怎么调⽤的。

Class 内部调⽤new.target，返回当前Class。

需要注意的是，⼦类继承⽗类时，new.target 会返回⼦类。

##### Class的继承

###### 简介

Class 可以通过extends 关键字实现继承，这⽐ES5 的通过修改原型链实现继承，要清晰和⽅便很多。

```
class Point{}
class COlorPoint extends Ponit{}
```

上⾯代码定义了⼀个ColorPoint 类，该类通过extends 关键字，继承了Point 类的所有属性和⽅法。

```
class ColorPoint extends Point {
	constructor(x, y, color) {
		super(x, y); // 调⽤⽗类的constructor(x, y)
		this.color = color;
	}
	toString() {
		return this.color + ' ' + super.toString(); // 调⽤⽗类的toString()
	}
}
```

上⾯代码中，constructor ⽅法和toString ⽅法之中，都出现了super 关键字，它在这⾥表示⽗类的构造函数，⽤来新建⽗类的this 对象。
⼦类必须在constructor ⽅法中调⽤super ⽅法，否则新建实例时会报错。这是因为⼦类没有⾃⼰的this 对象，⽽是继承⽗类的this 对象，然后对其进⾏加⼯。如果不调⽤super ⽅法，⼦类就得不到this 对象。

ES5 的继承，实质是先创造⼦类的实例对象this，然后再将⽗类的⽅法添加到this 上⾯（Parent.apply(this)）。ES6 的继承机制完全不同，实质是先创造⽗类的实例对象this（所以必须先调⽤super ⽅法），然后再⽤⼦类的构造函数修改this。
如果⼦类没有定义constructor ⽅法，这个⽅法会被默认添加，代码如下。也就是说，不管有没有显式定义，任何⼀个⼦类都有constructor⽅法。

另⼀个需要注意的地⽅是，在⼦类的构造函数中，只有调⽤super 之后，才可以使⽤this 关键字，否则会报错。这是因为⼦类实例的构建，是基于对⽗类实例加⼯，只有super ⽅法才能返回⽗类实例。

最后，⽗类的静态⽅法，也会被⼦类继承。

###### Object.getPrototypeOf()

Object.getPrototypeOf ⽅法可以⽤来从⼦类上获取⽗类。

```
Object.getPrototypeOf(ColorPoint) === Point
// true
```

因此，可以使⽤这个⽅法判断，⼀个类是否继承了另⼀个类。

###### super 关键字

super 这个关键字，既可以当作函数使⽤，也可以当作对象使⽤。在这两种情况下，它的⽤法完全不同。
第⼀种情况，super 作为函数调⽤时，代表⽗类的构造函数。ES6 要求，⼦类的构造函数必须执⾏⼀次super 函数。

```
class A {}
class B extends A {
	constructor() {
		super();
	}
}
```

上⾯代码中，⼦类B 的构造函数之中的super()，代表调⽤⽗类的构造函数。这是必须的，否则JavaScript 引擎会报错。
注意，super 虽然代表了⽗类A 的构造函数，但是返回的是⼦类B 的实例，即super 内部的this 指的是B，因此super() 在这⾥相当于A.prototype.constructor.call(this)。

作为函数时，super() 只能⽤在⼦类的构造函数之中，⽤在其他地⽅就会报错。

第⼆种情况，super 作为对象时，在普通⽅法中，指向⽗类的原型对象；在静态⽅法中，指向⽗类。

```
class A {
	p() {
		return 2;
	}
}
class B extends A {
	constructor() {
		super();
		console.log(super.p()); // 2
	}
}
let b = new B();
```

上⾯代码中，⼦类B 当中的super.p()，就是将super 当作⼀个对象使⽤。这时，super 在普通⽅法之中，指向A.prototype，所以super.p() 就相当于A.prototype.p()。

这⾥需要注意，由于super 指向⽗类的原型对象，所以定义在⽗类实例上的⽅法或属性，是⽆法通过super 调⽤的。如果属性定义在⽗类的原型对象上，super 就可以取到。

ES6 规定，通过super 调⽤⽗类的⽅法时，super 会绑定⼦类的this。

```
class A {
	constructor() {
		this.x = 1;
	}
	print() {
		console.log(this.x);
	}
}
class B extends A {
	constructor() {
		super();
		this.x = 2;
	} 
	m() {
		super.print();
	}
}
let b = new B();
b.m() // 2
```

上⾯代码中，super.print() 虽然调⽤的是A.prototype.print()，但是A.prototype.print() 会绑定⼦类B 的
this，导致输出的是2，⽽不是1。也就是说，实际上执⾏的是super.print.call(this)。

如果super 作为对象，⽤在静态⽅法之中，这时super 将指向⽗类，⽽不是⽗类的原型对象。super 在静态⽅法之中指向⽗类，在普通⽅法之中指向⽗类的原型对象。

注意，使⽤super 的时候，必须显式指定是作为函数、还是作为对象使⽤，否则会报错。

最后，由于对象总是继承其他对象的，所以可以在任意⼀个对象中，使⽤super 关键字。

###### 类的prototype 属性和__proto__ 属性

⼤多数浏览器的ES5 实现之中，每⼀个对象都有__ proto__ 属性，指向对应的构造函数的prototype 属性。Class 作为构造函数的语法糖，同时有prototype 属性和__ proto__ 属性，因此同时存在两条继承链。
（1）⼦类的__  proto__ 属性，表示构造函数的继承，总是指向⽗类。
（2）⼦类prototype 属性的__ proto__ 属性，表示⽅法的继承，总是指向⽗类的prototype 属性。

```
class A{
}
class B extends A {
}
B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```

这两条继承链，可以这样理解：作为⼀个对象，⼦类（B）的原型（__ proto__ 属性）是⽗类（A）；作为⼀个构造函数，⼦类（B）的原型对象（prototype 属性）是⽗类的原型对象（prototype 属性）的实例。

###### extends 的继承⽬标

extends 关键字后⾯可以跟多种类型的值。

class B extends A {
}
上⾯代码的A，只要是⼀个有prototype 属性的函数，就能被B 继承。由于函数都有prototype 属性（除了Function.prototype函数），因此A 可以是任意函数。
下⾯，讨论三种特殊情况。

第⼀种特殊情况，⼦类继承Object 类。

```
class A extends Object {
}
A.__proto__ === Object // true
A.prototype.__proto__ === Object.prototype // true
这种情况下，A 其实就是构造函数Object 的复制，A 的实例就是Object 的实例。
```

第⼆种特殊情况，不存在任何继承。

```
class A {
}
A.__proto__ === Function.prototype // true
A.prototype.__proto__ === Object.prototype // true
```

这种情况下，A 作为⼀个基类（即不存在任何继承），就是⼀个普通函数，所以直接继承Function.prototype。但是，A 调⽤后返回⼀个空对象（即Object 实例），所以A.prototype.__proto__ 指向构造函数（Object）的prototype 属性。

第三种特殊情况，⼦类继承null。

```
class A extends null {
}
A.__proto__ === Function.prototype // true
A.prototype.__proto__ === undefined // true
```

这种情况与第⼆种情况⾮常像。A 也是⼀个普通函数，所以直接继承Function.prototype。但是，A 调⽤后返回的对象不继承任何⽅法，所以它的__proto__ 指向Function.prototype，即实质上执⾏了下⾯的代码。
class C extends null {
	constructor() { return Object.create(null); }
}

###### 实例的__ proto__ 属性

⼦类实例的__ proto__ 属性的__ proto__ 属性，指向⽗类实例的__ proto__ 属性。也就是说，⼦类的原型的原型，是⽗类的原型。

###### 原⽣构造函数的继承

原⽣构造函数是指语⾔内置的构造函数，通常⽤来⽣成数据结构。ECMAScript 的原⽣构造函数⼤致有下⾯这些。
• Boolean()
• Number()
• String()
• Array()
• Date()
• Function()
• RegExp()
• Error()
• Object()

以前，这些原⽣构造函数是⽆法继承的。

ES5 是先新建⼦类的实例对象this，再将⽗类的属性添加到⼦类上，由于⽗类的内部属性⽆法获取，导致⽆法继承原⽣的构造函数。

ES6 允许继承原⽣构造函数定义⼦类，因为ES6 是先新建⽗类的实例对象this，然后再⽤⼦类的构造函数修饰this，使得⽗类的所有⾏为都可以继承。

ES6 可以⾃定义原⽣数据结构（⽐如Array、String 等）的⼦类，这是ES5 ⽆法做到的。
上⾯这个例⼦也说明，extends 关键字不仅可以⽤来继承类，还可以⽤来继承原⽣的构造函数。因此可以在原⽣数据结构的基础上，定义⾃⼰的数据结构。

###### Mixin 模式的实现

Mixin 指的是多个对象合成⼀个新的对象，新对象具有各个组成成员的接⼝。

##### 修饰器

###### 类的修饰

许多⾯向对象的语⾔都有修饰器（Decorator）函数，⽤来修改类的⾏为。⽬前，有⼀个提案将这项功能，引⼊了ECMAScript。

```
@decorator
class A {}
// 等同于
class A {}
A = decorator(A) || A;
```

也就是说，修饰器是⼀个对类进⾏处理的函数。修饰器函数的第⼀个参数，就是所要修饰的⽬标类。

注意，修饰器对类的⾏为的改变，是代码编译时发⽣的，⽽不是在运⾏时。这意味着，修饰器能在编译阶段运⾏代码。也就是说，修饰器本质就是编译时执⾏的函数。

实际开发中，React 与Redux 库结合使⽤时，常常需要写成下⾯这样。

class MyReactComponent extends React.Component {}
export default connect(mapStateToProps, mapDispatchToProps)(MyReactComponent);

有了装饰器，就可以改写上⾯的代码。

@connect(mapStateToProps, mapDispatchToProps)
export default class MyReactComponent extends React.Component {}

###### 方法的修饰

修饰器不仅可以修饰类，还可以修饰类的属性。

```
class Person {
	@readonly
	name() { return `${this.first} ${this.last}` }
}
```

上⾯代码中，修饰器readonly ⽤来修饰“类” 的name ⽅法。
此时，修饰器函数⼀共可以接受三个参数，第⼀个参数是所要修饰的⽬标对象，即类的实例（这不同于类的修饰，那种情况时target 参数指的是类本身）；第⼆个参数是所要修饰的属性名，第三个参数是该属性的描述对象。

```
function readonly(target, name, descriptor){
// descriptor 对象原来的值如下
// {
// value: specifiedFunction,
// enumerable: false,
// configurable: true,
// writable: true
// };
descriptor.writable = false;
return descriptor;
}
readonly(Person.prototype, 'name', descriptor);
// 类似于
Object.defineProperty(Person.prototype, 'name', descriptor);
```

上⾯代码说明，修饰器（readonly）会修改属性的描述对象（descriptor），然后被修改的描述对象再⽤来定义属性。

修饰器有注释的作⽤。

如果同⼀个⽅法有多个修饰器，会像剥洋葱⼀样，先从外到内进⼊，然后由内向外执⾏。

除了注释，修饰器还能⽤来类型检查。所以，对于类来说，这项功能相当有⽤。从⻓期来看，它将是JavaScript 代码静态分析的重要⼯具。

###### 为什么修饰器不能⽤于函数？

修饰器只能⽤于类和类的⽅法，不能⽤于函数，因为存在函数提升。

总之，由于存在函数提升，使得修饰器不能⽤于函数。类是不会提升的，所以就没有这⽅⾯的问题。
另⼀⽅⾯，如果⼀定要修饰函数，可以采⽤⾼阶函数的形式直接执⾏。

###### core-decorators.js

core-decorators.js是⼀个第三⽅模块，提供了⼏个常⻅的修饰器，通过它可以更好地理解修饰器。
（1）@autobind
autobind 修饰器使得⽅法中的this 对象，绑定原始对象。

（2）@readonly
readonly 修饰器使得属性或⽅法不可写。

（3）@override
override 修饰器检查⼦类的⽅法，是否正确覆盖了⽗类的同名⽅法，如果不正确会报错。

（4）@deprecate (别名@deprecated)
deprecate 或deprecated 修饰器在控制台显示⼀条警告，表示该⽅法将废除。

（5）@suppressWarnings
suppressWarnings 修饰器抑制deprecated 修饰器导致的console.warn() 调⽤。但是，异步代码发出的调⽤除外。

###### Mixin

在修饰器的基础上，可以实现Mixin 模式。所谓Mixin 模式，就是对象继承的⼀种替代⽅案，中⽂译为“混⼊”（mix in），意为在⼀个对象之中混⼊另外⼀个对象的⽅法。

###### Trait

Trait 也是⼀种修饰器，效果与Mixin 类似，但是提供更多功能，⽐如防⽌同名⽅法的冲突、排除混⼊某些⽅法、为混⼊的⽅法起别名等等。
下⾯采⽤traits-decorator这个第三⽅模块作为例⼦。这个模块提供的traits 修饰器，不仅可以接受对象，还可以接受ES6 类作为参数。

###### Babel 转码器的⽀持

⽬前，Babel 转码器已经⽀持Decorator。

##### Module的语法

###### 概述

历史上，JavaScript ⼀直没有模块（module）体系，⽆法将⼀个⼤程序拆分成互相依赖的⼩⽂件，再⽤简单的⽅法拼装起来。其他语⾔都有这项功能，⽐如Ruby 的require、Python 的import，甚⾄就连CSS 都有@import，但是JavaScript 任何这⽅⾯的⽀持都没有，这对开发⼤型的、复杂的项⽬形成了巨⼤障碍。
在ES6 之前，社区制定了⼀些模块加载⽅案，最主要的有CommonJS 和AMD 两种。前者⽤于服务器，后者⽤于浏览器。ES6 在语⾔标准的层⾯上，实现了模块功能，⽽且实现得相当简单，完全可以取代CommonJS 和AMD 规范，成为浏览器和服务器通⽤的模块解决⽅案。

ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输⼊和输出的变量。CommonJS 和AMD 模块，都只能在运⾏时确定这些东⻄。⽐如，CommonJS 模块就是对象，输⼊时必须查找对象属性。

ES6 模块不是对象，⽽是通过export 命令显式指定输出的代码，再通过import 命令输⼊。

```
// ES6 模块
import { stat, exists, readFile } from 'fs';
```

上⾯代码的实质是从fs 模块加载3 个⽅法，其他⽅法不加载。这种加载称为“编译时加载” 或者静态加载，即ES6 可以在编译时就完成模块加载，效率要⽐CommonJS 模块的加载⽅式⾼。当然，这也导致了没法引⽤ES6 模块本身，因为它不是对象。

由于ES6 模块是编译时加载，使得静态分析成为可能。有了它，就能进⼀步拓宽JavaScript 的语法，⽐如引⼊宏（macro）和类型检验（type system）这些只能靠静态分析实现的功能。
除了静态加载带来的各种好处，ES6 模块还有以下好处。
• 不再需要UMD 模块格式了，将来服务器和浏览器都会⽀持ES6 模块格式。⽬前，通过各种⼯具库，其实已经做到了这⼀点。
• 将来浏览器的新API 就能⽤模块格式提供，不再必须做成全局变量或者navigator 对象的属性。
• 不再需要对象作为命名空间（⽐如Math 对象），未来这些功能可以通过模块提供。

###### 严格模式

ES6 的模块⾃动采⽤严格模式，不管你有没有在模块头部加上"use strict";。
严格模式主要有以下限制。
• 变量必须声明后再使⽤
• 函数的参数不能有同名属性，否则报错
• 不能使⽤with 语句
• 不能对只读属性赋值，否则报错
• 不能使⽤前缀0 表示⼋进制数，否则报错
• 不能删除不可删除的属性，否则报错
• 不能删除变量delete prop，会报错，只能删除属性delete global[prop]
• eval 不会在它的外层作⽤域引⼊变量
• eval 和arguments 不能被重新赋值
• arguments 不会⾃动反映函数参数的变化
• 不能使⽤arguments.callee
• 不能使⽤arguments.caller
• 禁⽌this 指向全局对象
• 不能使⽤fn.caller 和fn.arguments 获取函数调⽤的堆栈
• 增加了保留字（⽐如protected、static 和interface）
上⾯这些限制，模块都必须遵守。由于严格模式是ES5 引⼊的，不属于ES6，所以请参阅相关ES5 书籍，本书不再详细介绍了。
其中，尤其需要注意this 的限制。ES6 模块之中，顶层的this 指向undefined，即不应该在顶层代码使⽤this。

###### export 命令

模块功能主要由两个命令构成：export 和import。export 命令⽤于规定模块的对外接⼝，import 命令⽤于输⼊其他模块提供的功能。
⼀个模块就是⼀个独⽴的⽂件。该⽂件内部的所有变量，外部⽆法获取。如果你希望外部能够读取模块内部的某个变量，就必须使⽤export 关键字输出该变量。

```
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;

// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;
export {firstName, lastName, year};
```

上⾯代码在export 命令后⾯，使⽤⼤括号指定所要输出的⼀组变量。它与前⼀种写法（直接放置在var 语句前）是等价的，但是应该优先考虑使⽤这种写法。因为这样就可以在脚本尾部，⼀眼看清楚输出了哪些变量。

export 命令除了输出变量，还可以输出函数或类（class）。

通常情况下，export 输出的变量就是本来的名字，但是可以使⽤as 关键字重命名。

```
function v1() { ... }
function v2() { ... }
export {
	v1 as streamV1,
	v2 as streamV2,
	v2 as streamLatestVersion
};
```

上⾯代码使⽤as 关键字，重命名了函数v1 和v2 的对外接⼝。重命名后，v2 可以⽤不同的名字输出两次。

需要特别注意的是，export 命令规定的是对外的接⼝，必须与模块内部的变量建⽴⼀⼀对应关系。

```
// 报错
export 1;
// 报错
var m = 1;
export m;
```

上⾯两种写法都会报错，因为没有提供对外的接⼝。第⼀种写法直接输出1，第⼆种写法通过变量m，还是直接输出1。1 只是⼀个值，不是接⼝。
正确的写法是下⾯这样。

```
// 写法⼀
export var m = 1;
// 写法⼆
var m = 1;
export {m};
// 写法三
var n = 1;
export {n as m};
```

上⾯三种写法都是正确的，规定了对外的接⼝m。其他脚本可以通过这个接⼝，取到值1。它们的实质是，在接⼝名与模块内部变量之间，建⽴了⼀⼀对应的关系。
同样的，function 和class 的输出，也必须遵守这样的写法。

另外，export 语句输出的接⼝，与其对应的值是动态绑定关系，即通过该接⼝，可以取到模块内部实时的值。

这⼀点与CommonJS 规范完全不同。CommonJS 模块输出的是值的缓存，不存在动态更新。

最后，export 命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作⽤域内，就会报错，下⼀节的import 命令也是如此。这是因为处于条件代码块之中，就没法做静态优化了，违背了ES6 模块的设计初衷。

###### import 命令

使⽤export 命令定义了模块的对外接⼝以后，其他JS ⽂件就可以通过import 命令加载这个模块。

```
// main.js
import {firstName, lastName, year} from './profile';
function setName(element) {
	element.textContent = firstName + ' ' + lastName;
}
```

上⾯代码的import 命令，⽤于加载profile.js ⽂件，并从中输⼊变量。import 命令接受⼀对⼤括号，⾥⾯指定要从其他模块导⼊的变量名。⼤括号⾥⾯的变量名，必须与被导⼊模块（profile.js）对外接⼝的名称相同。
如果想为输⼊的变量重新取⼀个名字，import 命令要使⽤as 关键字，将输⼊的变量重命名。

```
import { lastName as surname } from './profile';
```

import 后⾯的from 指定模块⽂件的位置，可以是相对路径，也可以是绝对路径，.js 后缀可以省略。如果只是模块名，不带有路径，那么必须有配置⽂件，告诉JavaScript 引擎该模块的位置。

注意，import 命令具有提升效果，会提升到整个模块的头部，⾸先执⾏。

```
foo();
import { foo } from 'my_module';
```

上⾯的代码不会报错，因为import 的执⾏早于foo 的调⽤。这种⾏为的本质是，import 命令是编译阶段执⾏的，在代码运⾏之前。
由于import 是静态执⾏，所以不能使⽤表达式和变量，这些只有在运⾏时才能得到结果的语法结构。

最后，import 语句会执⾏所加载的模块，因此可以有下⾯的写法。

```
import 'lodash';
上⾯代码仅仅执⾏lodash 模块，但是不输⼊任何值。
如果多次重复执⾏同⼀句import 语句，那么只会执⾏⼀次，⽽不会执⾏多次。
import 'lodash';
import 'lodash';
上⾯代码加载了两次lodash，但是只会执⾏⼀次。
import { foo } from 'my_module';
import { bar } from 'my_module';
// 等同于
import { foo, bar } from 'my_module';
```

上⾯代码中，虽然foo 和bar 在两个语句中加载，但是它们对应的是同⼀个my_module 实例。也就是说，import 语句是Singleton模式。

⽬前阶段，通过Babel 转码，CommonJS 模块的require 命令和ES6 模块的import 命令，可以写在同⼀个模块⾥⾯，但是最好不要这样做。因为import 在静态解析阶段执⾏，所以它是⼀个模块之中最早执⾏的。

###### 模块的整体加载

除了指定加载某个输出值，还可以使⽤整体加载，即⽤星号（*）指定⼀个对象，所有输出值都加载在这个对象上⾯。

```
import { area, circumference } from './circle';

上⾯写法是逐⼀指定要加载的⽅法，整体加载的写法如下。
import * as circle from './circle';
```

注意，模块整体加载所在的那个对象（上例是circle），应该是可以静态分析的，所以不允许运⾏时改变。

###### export default 命令

从前⾯的例⼦可以看出，使⽤import 命令的时候，⽤户需要知道所要加载的变量名或函数名，否则⽆法加载。但是，⽤户肯定希望快速上⼿，未必愿意阅读⽂档，去了解模块有哪些属性和⽅法。
为了给⽤户提供⽅便，让他们不⽤阅读⽂档就能加载模块，就要⽤到export default 命令，为模块指定默认输出。

```
// 第⼀组
export default function crc32() { // 输出
// ...
}
import crc32 from 'crc32'; // 输⼊

// 第⼆组
export function crc32() { // 输出
// ...
};
import {crc32} from 'crc32'; // 输⼊
```

上⾯代码的两组写法，第⼀组是使⽤export default 时，对应的import 语句不需要使⽤⼤括号；第⼆组是不使⽤export default 时，对应的import 语句需要使⽤⼤括号。
export default 命令⽤于指定模块的默认输出。显然，⼀个模块只能有⼀个默认输出，因此export default 命令只能使⽤⼀次。所以，import 命令后⾯才不⽤加⼤括号，因为只可能对应⼀个⽅法。

本质上，export default 就是输出⼀个叫做default 的变量或⽅法，然后系统允许你为它取任意名字。

```
function add(x, y) {
	return x * y;
}
export {add as default};
// 等同于
// export default add;
// app.js
import { default as foo } from 'modules';
// 等同于
// import foo from 'modules';
```

正是因为export default 命令其实只是输出⼀个叫做default 的变量，所以它后⾯不能跟变量声明语句。

```
// 正确
export var a = 1;
// 正确
var a = 1;
export default a;
// 错误
export default var a = 1;
```

###### export 与import 的复合写法

如果在⼀个模块之中，先输⼊后输出同⼀个模块，import 语句可以与export 语句写在⼀起。

```
export { foo, bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';
export { foo, bar };
```

上⾯代码中，export 和import 语句可以结合在⼀起，写成⼀⾏。

###### 模块的继承

模块之间也可以继承。

###### 跨模块常量

本书介绍const 命令的时候说过，const 声明的常量只在当前代码块有效。如果想设置跨模块的常量（即跨多个⽂件），或者说⼀个值要被多个模块共享，可以采⽤下⾯的写法。

```
// constants.js 模块
export const A = 1;
export const B = 3;
export const C = 4;
```

##### Module的加载实现

###### 浏览器加载

传统方法

在HTML ⽹⻚中，浏览器通过<script> 标签加载JavaScript 脚本。
<!-- ⻚⾯内嵌的脚本-->

<script type="application/javascript">
// module code
</script>

<!-- 外部脚本-->

<script type="application/javascript" src="path/to/myModule.js">
</script>

上⾯代码中，由于浏览器脚本的默认语⾔是JavaScript，因此type="application/javascript" 可以省略。
默认情况下，浏览器是同步加载JavaScript 脚本，即渲染引擎遇到<script> 标签就会停下来，等到执⾏完脚本，再继续向下渲染。如果是外部脚本，还必须加⼊脚本下载的时间。
如果脚本体积很⼤，下载和执⾏的时间就会很⻓，因此造成浏览器堵塞，⽤户会感觉到浏览器“卡死” 了，没有任何响应。这显然是很不好的体验，所以浏览器允许脚本异步加载，下⾯就是两种异步加载的语法。

<script src="path/to/myModule.js" defer></script>
<script src="path/to/myModule.js" async></script>

上⾯代码中，<script> 标签打开defer 或async 属性，脚本就会异步加载。渲染引擎遇到这⼀⾏命令，就会开始下载外部脚本，但不会等它下载和执⾏，⽽是直接执⾏后⾯的命令。
defer 与async 的区别是：前者要等到整个⻚⾯正常渲染结束，才会执⾏；后者⼀旦下载完，渲染引擎就会中断渲染，执⾏这个脚本以后，再继续渲染。⼀句话，defer 是“渲染完再执⾏”，async 是“下载完就执⾏”。另外，如果有多个defer 脚本，会按照它们在⻚⾯出现的顺序加载，⽽多个async 脚本是不能保证加载顺序的。

加载规则

浏览器加载ES6 模块，也使⽤<script> 标签，但是要加⼊type="module" 属性。

<script type="module" src="./foo.js"></script>

上⾯代码在⽹⻚中插⼊⼀个模块foo.js，由于type 属性设为module，所以浏览器知道这是⼀个ES6 模块。
浏览器对于带有type="module" 的<script>，都是异步加载，不会造成堵塞浏览器，即等到整个⻚⾯渲染完，再执⾏模块脚本，等同于打开了<script> 标签的defer 属性。

<script type="module" src="./foo.js"></script>

<!-- 等同于-->

<script type="module" src="./foo.js" defer></script>

如果⽹⻚有多个<script type="module">，它们会按照在⻚⾯出现的顺序依次执⾏。

<script> 标签的async 属性也可以打开，这时只要加载完成，渲染引擎就会中断渲染⽴即执⾏。执⾏完成后，再恢复渲染。
<script type="module" src="./foo.js" async></script>
⼀旦使⽤了async 属性，<script type="module"> 就不会按照在⻚⾯出现的顺序执⾏，⽽是只要该模块加载完成，就执⾏该模块。

###### ES6 模块与CommonJS 模块的差异

它们有两个重⼤差异。
• CommonJS 模块输出的是⼀个值的拷⻉，ES6 模块输出的是值的引⽤。
• CommonJS 模块是运⾏时加载，ES6 模块是编译时输出接⼝。

第⼆个差异是因为CommonJS 加载的是⼀个对象（即module.exports 属性），该对象只有在脚本运⾏完才会⽣成。⽽ES6 模块不是对象，它的对外接⼝只是⼀种静态定义，在代码静态解析阶段就会⽣成。

CommonJS 模块输出的是值的拷⻉，也就是说，⼀旦输出⼀个值，模块内部的变化就影响不到这个值。

ES6 模块的运⾏机制与CommonJS 不⼀样。JS 引擎对脚本静态分析的时候，遇到模块加载命令import，就会⽣成⼀个只读引⽤。等到脚本真正执⾏时，再根据这个只读引⽤，到被加载的那个模块⾥⾯去取值。换句话说，ES6 的import 有点像Unix 系统的“符号连接”，原始值变了，import 加载的值也会跟着变。因此，ES6 模块是动态引⽤，并且不会缓存值，模块⾥⾯的变量绑定其所在的模块。

ES6 模块不会缓存运⾏结果，⽽是动态地去被加载的模块取值，并且变量总是绑定其所在的模块。
由于ES6 输⼊的模块变量，只是⼀个“符号连接”，所以这个变量是只读的，对它进⾏重新赋值会报错。

最后，export 通过接⼝，输出的是同⼀个值。不同的脚本加载这个接⼝，得到的都是同样的实例。

###### Node加载

Node 对ES6 模块的处理⽐较麻烦，因为它有⾃⼰的CommonJS 模块格式，与ES6 模块格式是不兼容的。⽬前的解决⽅案是，将两者分开，ES6 模块和CommonJS 采⽤各⾃的加载⽅案。
Node 要求ES6 模块采⽤.mjs 后缀⽂件名。也就是说，只要脚本⽂件⾥⾯使⽤import 或者export 命令，那么就必须采⽤.mjs后缀名。require 命令不能加载.mjs ⽂件，会报错，只有import 命令才可以加载.mjs ⽂件。反过来，.mjs ⽂件⾥⾯也不能使⽤require 命令，必须使⽤import。
⽬前，这项功能还在试验阶段。

###### 循环加载

“循环加载”（circular dependency）指的是，a 脚本的执⾏依赖b 脚本，⽽b 脚本的执⾏⼜依赖a 脚本。

###### CommonJS 模块的加载原理

CommonJS 的⼀个模块，就是⼀个脚本⽂件。require 命令第⼀次加载该脚本，就会执⾏整个脚本，然后在内存⽣成⼀个对象。

以后需要⽤到这个模块的时候，就会到exports 属性上⾯取值。即使再次执⾏require 命令，也不会再次执⾏该模块，⽽是到缓存之中取值。也就是说，CommonJS 模块⽆论加载多少次，都只会在第⼀次加载时运⾏⼀次，以后再加载，就返回第⼀次运⾏的结果，除⾮⼿动清除系统缓存。

###### CommonJS 模块的循环加载

CommonJS 模块的重要特性是加载时执⾏，即脚本代码在require 的时候，就会全部执⾏。⼀旦出现某个模块被“循环加载”，就只输出已经执⾏的部分，还未执⾏的部分不会输出。

总之，CommonJS 输⼊的是被输出值的拷⻉，不是引⽤。
另外，由于CommonJS 模块遇到循环加载时，返回的是当前已经执⾏的部分的值，⽽不是代码全部执⾏后的值，两者可能会有差异。所以，输⼊变量的时候，必须⾮常⼩⼼。

###### ES6 模块的循环加载

ES6 处理“循环加载” 与CommonJS 有本质的不同。ES6 模块是动态引⽤，如果使⽤import 从⼀个模块加载变量（即import foo from 'foo'），那些变量不会被缓存，⽽是成为⼀个指向被加载模块的引⽤，需要开发者⾃⼰保证，真正取值的时候能够取到值。

原因就在于ES6 加载的变量，都是动态引⽤其所在的模块。只要引⽤存在，代码就能执⾏。

###### ES6 模块的转码

浏览器⽬前还不⽀持ES6 模块，为了现在就能使⽤，可以将转为ES5 的写法。除了Babel 可以⽤来转码之外，还有以下两个⽅法，也可以⽤来转码。

ES6 module transpiler是square 公司开源的⼀个转码器，可以将ES6 模块转为CommonJS 模块或AMD 模块的写法，从⽽在浏览器中使⽤。

另⼀种解决⽅法是使⽤SystemJS。它是⼀个垫⽚库（polyfill），可以在浏览器内加载ES6 模块、AMD 模块和CommonJS 模块，将其转为ES5 格式。它在后台调⽤的是Google 的Traceur 转码器。

##### 编码风格

###### 块级作用域

1.let取代var

ES6 提出了两个新的声明变量的命令：let 和const。其中，let 完全可以取代var，因为两者语义相同，⽽且let 没有副作⽤。

var 命令存在变量提升效⽤，let 命令没有这个问题。所以，建议不再使⽤var 命令，⽽是使⽤let 命令取代。

2.全局常量和线程安全

在let 和const 之间，建议优先使⽤const，尤其是在全局环境，不应该设置变量，只应设置常量。

const 优于let 有⼏个原因。⼀个是const 可以提醒阅读程序的⼈，这个变量不应该改变；另⼀个是const ⽐较符合函数式编程思想，运算不改变值，只是新建值，⽽且这样也有利于将来的分布式运算；最后⼀个原因是JavaScript 编译器会对const 进⾏优化，所以多使⽤const，有利于提供程序的运⾏效率，也就是说let 和const 的本质区别，其实是编译器内部的处理不同。

const 声明常量还有两个好处，⼀是阅读代码的⼈⽴刻会意识到不应该修改这个值，⼆是防⽌了⽆意间修改变量值所导致的错误。
所有的函数都应该设置为常量。
⻓远来看，JavaScript 可能会有多线程的实现（⽐如Intel 公司的River Trail 那⼀类的项⽬），这时let 表示的变量，只应出现在单线程运⾏的代码中，不能是多线程共享的，这样有利于保证线程安全。

###### 字符串

静态字符串⼀律使⽤单引号或反引号，不使⽤双引号。动态字符串使⽤反引号。

###### 解构赋值

使⽤数组成员对变量赋值时，优先使⽤解构赋值。

函数的参数如果是对象的成员，优先使⽤解构赋值。

如果函数返回多个值，优先使⽤对象的解构赋值，⽽不是数组的解构赋值。这样便于以后添加返回值，以及更改返回值的顺序。

###### 对象

单⾏定义的对象，最后⼀个成员不以逗号结尾。多⾏定义的对象，最后⼀个成员以逗号结尾。

对象尽量静态化，⼀旦定义，就不得随意添加新的属性。如果添加属性不可避免，要使⽤Object.assign ⽅法。

如果对象的属性名是动态的，可以在创造对象的时候，使⽤属性表达式定义。

另外，对象的属性和⽅法，尽量采⽤简洁表达法，这样易于描述和书写。

###### 数组

使⽤扩展运算符（…）拷⻉数组。

使⽤Array.from ⽅法，将类似数组的对象转为数组。

###### 函数

⽴即执⾏函数可以写成箭头函数的形式。

箭头函数取代Function.prototype.bind，不应再⽤self/_this/that 绑定this。

简单的、单⾏的、不会复⽤的函数，建议采⽤箭头函数。如果函数体较为复杂，⾏数较多，还是应该采⽤传统的函数写法。
所有配置项都应该集中在⼀个对象，放在最后⼀个参数，布尔值不可以直接作为参数。

不要在函数体内使⽤arguments 变量，使⽤rest 运算符（…）代替。因为rest 运算符显式表明你想要获取参数，⽽且arguments 是⼀个类似数组的对象，⽽rest 运算符可以提供⼀个真正的数组。

###### Map 结构

注意区分Object 和Map，只有模拟现实世界的实体对象时，才使⽤Object。如果只是需要key: value 的数据结构，使⽤Map 结构。因为Map 有内建的遍历机制。

###### Class

总是⽤Class，取代需要prototype 的操作。因为Class 的写法更简洁，更易于理解。

使⽤extends 实现继承，因为这样更简单，不会有破坏instanceof 运算的危险。

###### 模块

⾸先，Module 语法是JavaScript 模块的标准写法，坚持使⽤这种写法。使⽤import 取代require。

使⽤export 取代module.exports。

如果模块只有⼀个输出值，就使⽤export default，如果模块有多个输出值，就不使⽤export default，export default 与普通的export 不要同时使⽤。
不要在模块输⼊中使⽤通配符。因为这样可以确保你的模块之中，有⼀个默认输出（export default）。

如果模块默认输出⼀个函数，函数名的⾸字⺟应该⼩写。

如果模块默认输出⼀个对象，对象名的⾸字⺟应该⼤写。

###### ESLint 的使⽤

ESLint 是⼀个语法规则和代码⻛格的检查⼯具，可以⽤来保证写出语法正确、⻛格统⼀的代码。

##### 读懂ECMAScript 规格

###### 概述

规格⽂件是计算机语⾔的官⽅标准，详细描述语法规则和实现⽅法。
⼀般来说，没有必要阅读规格，除⾮你要写编译器。因为规格写得⾮常抽象和精炼，⼜缺乏实例，不容易理解，⽽且对于解决实际的应⽤问题，帮助不⼤。但是，如果你遇到疑难的语法问题，实在找不到答案，这时可以去查看规格⽂件，了解语⾔标准是怎么说的。规格是解决问题的“最后⼀招”。
这对JavaScript 语⾔很有必要。因为它的使⽤场景复杂，语法规则不统⼀，例外很多，各种运⾏环境的⾏为不⼀致，导致奇怪的语法问题层出不穷，任何语法书都不可能囊括所有情况。查看规格，不失为⼀种解决语法问题的最可靠、最权威的终极⽅法。

##### ArrayBuffer

ArrayBuffer 对象、TypedArray 视图和DataView 视图是JavaScript 操作⼆进制数据的⼀个接⼝。这些对象早就存在，属于独⽴的规格（2011 年2 ⽉发布），ES6 将它们纳⼊了ECMAScript 规格，并且增加了新的⽅法。它们都是以数组的语法处理⼆进制数据，所以统称为⼆进制数组。

所谓WebGL，就是指浏览器与显卡之间的通信接⼝，为了满⾜JavaScript 与显卡之间⼤量的、实时的数据交换，它们之间的数据通信必须是⼆进制的，⽽不能是传统的⽂本格式。⽂本格式传递⼀个32 位整数，两端的JavaScript 脚本与显卡都要进⾏格式转化，将⾮常耗时。这时要是存在⼀种机制，可以像C 语⾔那样，直接操作字节，将4 个字节的32 位整数，以⼆进制形式原封不动地送⼊显卡，脚本的性能就会⼤幅提升。
⼆进制数组就是在这种背景下诞⽣的。它很像C 语⾔的数组，允许开发者以数组下标的形式，直接操作内存，⼤⼤增强了JavaScript 处理⼆进制数据的能⼒，使得开发者有可能通过JavaScript 与操作系统的原⽣接⼝进⾏⼆进制通信。

⼆进制数组由三类对象组成。
（1）ArrayBuffer 对象：代表内存之中的⼀段⼆进制数据，可以通过“视图” 进⾏操作。“视图” 部署了数组接⼝，这意味着，可以⽤数组的⽅法操作内存。
（2）TypedArray 视图：共包括9 种类型的视图，⽐如Uint8Array（⽆符号8 位整数）数组视图, Int16Array（16 位整数）数组视图, Float32Array（32 位浮点数）数组视图等等。
（3）DataView 视图：可以⾃定义复合格式的视图，⽐如第⼀个字节是Uint8（⽆符号8 位整数）、第⼆、三个字节是Int16（16 位整数）、第四个字节开始是Float32（32 位浮点数）等等，此外还可以⾃定义字节序。
简单说，ArrayBuffer 对象代表原始的⼆进制数据，TypedArray 视图⽤来读写简单类型的⼆进制数据，DataView 视图⽤来读写复杂类型的⼆进制数据。

注意，⼆进制数组并不是真正的数组，⽽是类似数组的对象。
很多浏览器操作的API，⽤到了⼆进制数组操作⼆进制数据，下⾯是其中的⼏个。
• File API
• XMLHttpRequest
• Fetch API
• Canvas
• WebSockets

###### 二进制数组的应用

1.AJAX

传统上，服务器通过AJAX 操作只能返回⽂本数据，即responseType 属性默认为text。XMLHttpRequest 第⼆版XHR2 允许服务器返回⼆进制数据，这时分成两种情况。如果明确知道返回的⼆进制数据类型，可以把返回类型（responseType）设为arraybuffer；如果不知道，就设为blob。

2.Canvas

⽹⻚Canvas 元素输出的⼆进制像素数据，就是TypedArray 数组。

3.WebSocket

WebSocket 可以通过ArrayBuffer，发送或接收⼆进制数据。

4.Fetch API

Fetch API 取回的数据，就是ArrayBuffer 对象。

5.File API

如果知道⼀个⽂件的⼆进制数据类型，也可以将这个⽂件读取为ArrayBuffer 对象。
