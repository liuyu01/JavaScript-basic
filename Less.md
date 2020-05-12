##### sass和less的区别

Less(Leaner Style Sheets的缩写)是一门向后兼容的CSS扩展语言。因为Less和CSS非常像，Less仅对CSS语言增加了少许方便的扩展，学习很容易。

sass，作为“世界上最成熟、最稳定、最强大的专业级CSS扩展语言”。兼容所有版本的css，且有无数框架使用sass构建，如Compass,Bourbn，和Susy。

相同之处：

1.混入（Mixins）--class中的class

2.参数混入--可以传递参数的class，就像函数一样

3.嵌套规则--Class中嵌套class，从而减少重复的代码

4.运算--CSS中用上数学

5.颜色功能--可以编辑颜色

6.名字空间（namespace）--分组样式，从而可以被调用

7.作用域--局部修改样式

8.JavaScript赋值--在CSS中使用JavaScript表达式赋值

两者异同：

类别                 Sass                                            Less

安装         sass的安装需要安装Ruby环境        Less基于JavaScript，是需要引入Less.js来处理代码输出css到浏览器，也可以在开发环节使用Less,然后编译成css文件，直接放在项目中

使用              复杂                                                    简单

功能              复杂                                                    简单

处理机制      通过服务端处理                                 通过客户端处理的，解析会比sass慢一点

变量              以$开始                                               以@开始

文件后缀      .sass或.scss                                        .less

1.Less在JS上允许，Sass在Ruby上使用

Sass基于Ruby，需要安装Ruby。Less和Sass在Ruby中构建相似，但它已被移植到JavaScript中。为了使用Less,我们可以将适用的JavaScript文件上载到服务器或通过脱机编译器编译CSS表。

2.编写变量的方式不同

Sass使用$，而Less使用@

3.在Less中，仅允许循环数值

在Sass中，我们可以遍历任何类型的数据。但在Less中，我们只能使用递归函数循环数值。

4.Sass有Compass，Less有Preboot

Sass和Less有可用于集成mixins的扩展（在整个站点中存储和共享CSS声明的能力）。Sass有适用于mixins的Compass，其中包括所有可用的选项以及未来支持的更新。

Less有Preboot.less，LESS Mixins，LESS Elements，gs和Frameless。LESS的软件支持比Sass更加分散，导致许多不同的扩展选项可能不会以相同的方式运行。对于项目，我们可能需要所有列出的扩展以获得与Compass类似的性能。

##### 什么是LESS?

LESS是一个CSS预处理器，可以为网站启用可自定义，可管理和可重用的样式表。LESS是一种动态样式表语言，扩展了CSS的功能。LESS也是跨浏览器友好。CSS预处理器是一种脚本语言，可扩展CSS并将其编译为常规CSS语法，以便可以通过web浏览器读取。它提供诸如变量，函数，mixins和操作等功能，可以构建动态CSS。

##### 为什么要使用LESS?

- LESS支持创建更清洁，跨浏览器友好的CSS更快更容易。
- LESS是使用JavaScript设计的，并且创建在live中使用，其编译速度比其他CSS预处理器更快。
- LESS保持你的代码以模块化的方式，这是非常重要的，通过使其可读性和容易改变。
- 可以通过使用LESS变量来实现更快的维护。

##### 特征

- 更清晰和更可读的代码可以以有组织的方式编写
- 我们可以定义样式，它可以在整个代码中重复使用
- LESS是基于JavaScript的，是超集的CSS
- LESS是一个敏捷工具，可以排除代码冗余的问题

##### 优点

- LESS轻松地生成可在浏览器中工作的CSS
- LESS使您能够使用嵌套编写更干净，组织良好的代码
- 通过使用变量可以更快地实现维护
- LESS使您能够通过在规则集中引用它们来轻松地重用整个类
- LESS提供使用操作，使得编码更快并节省时间

##### 缺点

- 学习如果你是新的CSS预处理器需要时间
- 由于模块之间的紧密耦合，应当采取更多的努力来重用和/或测试依赖模块
- 与旧的预处理器（例如Sass）相比，LESS具有较少的框架，Sass由框架Compass,Gravity和Susy组成

Less(Leaner Style Sheets的缩写)是一门向后兼容的CSS扩展语言。

##### 变量

```
@width: 10px;
@height: @width + 10px;
#header{
	width:@width;
	height:@height;
}
```

编译为：

```
#header{
	width: 10px;
	height: 20px;
}
```

##### 混合（Mixins）

混合是一种将一组属性从一个规则集包含（或混入）到另一个规则集的方法。

```
.bordered{
	border-top:dotted 1px black;
	border-bottom: solid 2px black;
}
```

如果我们希望在其他规则集中使用这些属性呢？

```
#menu a {
	color: #111;
	.bordered();
}
.post a{
	color: red;
	.bordered();
}
```

.bordered类所包含的属性就将同时出现在#menu a 和 .post a中 了。

##### 嵌套（Nesting）

Less提供了使用嵌套（nesting）代替层叠或与层叠结合使用的能力。假设我们有以下css代码：

```
#header{
	color: black;
}
#header .navigation{
	font-size:12px;
}
#header .logo{
	width:300px;
}
```

用Less语言我们可以这样书写代码：

```
#header{
	color:black;
	.navigation{
		font-size:12px;
	}
	.logo{
		width:300px;
	}
}
```

用Less书写的代码更加简洁，并且模仿了HTML的组织结构。

##### @规则嵌套和冒泡

@规则（例如@media 或@supports）可以与选择器以相同的方式进行嵌套。@规则会被放在前面，同一规则集中的其他元素的相对顺序保持不变。这叫做冒泡（bubbling）。

```
.component{
	width:300px;
	@media(min-width:768px){
		width:600px;
		@media(min-resolution:192dpi){
			background-image:url(/img/retina2x.png);
		}
	}
	@media(min-width:1280px){
		width:800px;
	}
}
```

编译为：

```
.component{
	width:300px;
}
@media(min-width:768px){
	.component{
		width:600px;
	}
}
@media(min-width:768px) and (min-resolution:192dpi){
	.component{
		background-image:url(/img/retina2x.png);
	}
}
@media(min-width:1280px){
	.component{
		width:800px;
	}
}
```

##### 运算（Operations）

算术运算符+、-、*、/可以对任何数字、颜色或变量进行运算。如果可能的话，算术运算符在加、减或比较之前会进行单位换算。计算的结果以最左侧操作数的单位类型为准。如果单位换算无效或失去意义，则忽略单位。无效的单位换算例如：px到cm或rad到%的转换。

```
//所有操作数被转换成相同的单位
@conversion-1: 5cm + 10mm; //6cm
@conversion-2: 2 - 3cm -5mm; //-1.5cm
//conversion is impossible
@incompatible-units: 2 + 5px -3cm;  //4px
//example with variables
@base: 5%;
@filter: @base *2 ;// 结果是10%
@other: @base + @filter;// 结果是15%

```

乘法和除法不作转换。因为这两种运算在大多数情况下都没有意义，一个长度乘以一个长度就得到一个区域，而CSS是不支持指定区域的。Less将按数字的原样进行操作，并将为计算结果指定明确的单位类型。

```
@base：2cm * 3mm; //6cm
```

你还可以对颜色进行算术运算：

```
@color: #224488/2;  //#112244
background-color: #112244 + #111; //#223355
```

calc()特例

为了与CSS保持兼容，查calc()并不对数学表达式进行计算，但是在嵌套函数中会计算变量和数学公式的值。

```
@var:50vh/2;
width:calc(50% + (@var - 20px));结果是calc(50% + (25vh - 20px))
```

##### 转义（Escaping）

转义允许是使用任何意义字符串作为属性或变量值。任何~"anything"或~‘anything’形式的内容都将按原样输出，除非interpolation。

```
@min768:~"(min-width:768px)";
.element{
	@media @min768{
		font-size:1.2rem;
	}
}
```

编译为：

```
@media (min-width:768px){
	.element{
		font-size:1.2rem;
	}
}
```

在Less3.5+版本中，许多以前需要“引号转义”的情况就不再需要了。在将Less代码编译为CSS代码之后，~’some_text‘中的任何内容将显示为some_text。

##### 函数（Functions）

Less内置了多种函数用于转换颜色、处理字符串、算术运算等。

函数的用法非常简单。下面这个例子将介绍如何利用percentage函数将0.5转换为50%，将颜色饱和度增加5%，以及颜色亮度降低25%并且色相值增加8等用法

```
@base: #f04615;
@width: 0.5;
.class{
	width:percentage(@width); //50%
	color:saturate(@base, 5%);
	background-color:spin(lighten(@base,25%),8)
}
```

##### 命名空间和访问符

不要和CSS @namespace 或namespace selectors混淆了。

有时，出于组织结构或仅仅是为了提供一些封装的目的，你希望对混合（mixins）进行分组。你可以用Less更直观地实现这一需求。假设你希望将一些混合（mixins）和变量重置于#bundle之下，为了以后方便重用或分发：

```
#bundle{
	.button{
		display: block;
		border: 1px solid black;
		background-color: grey;
		&:hover{
			background-color: white;
		}
	}
	.tab{}
	.citation{}
}
```

现在，如果我们希望把.button类混合到#header a中，我们可以这样做：

```
#header a{
	color: orange;
	#bundle.button(); //还可以书写为 #bundle > .button 形式
}
```

##### 映射（Maps）

从Less 3.5版本开始，你还可以将混合（mixins）和规则集（rulesets）作为一组值得映射（map）使用。

```
#colors(){
	primary: blue;
	secondary: green;
}
.button{
	color: #colors[primary];
	border: 1px solid #colors[secondary];
}
```

输出符合预期：

```
.button{
	color:blue;
	border: 1px solid green;
}
```

##### 作用域（Scope）

Less中的作用域与CSS中的作用域非常类似。首先在本地查找变量和混合（mixins），如果找不到，则从‘父’级作用域继承。

```
@var: red;
#page{
	@var: white;
	#header{
		color:@var;//white
	}
}
```

与CSS自定义属性一样，混合（mixins）和变量的定义不必在引用之前事先定义。因此，下面的Less代码示例和上面的代码示例是相同的：

```
@var: red;
#page{
	#header{
		color:@var;//white
	}
	@var: white;
}
```

##### 注释（Comments）

块注释和行注释都可以使用：

```
/*一个块注释
 *      */
 @var: red;
//这一行被注释掉了
@var: white;
```

##### 导入（Importing）

导入的工作方式和你预期的一样。你可以导入一个.less文件，此文件中的所有变量就可以全部使用了。如果导入的文件是.less扩展名，则可以将扩展名沈略掉：

```
@import "library"; //library.less
@import "typo.css";
```

##### 扩展

Extend是一个Less伪类，它通过使用:extend选择器在一个选择器中扩展其他选择器样式。

```
h2{
 &:extend(.style);
 font-size: italic;
}
.style{
  background: green;
}
```

编译为：

```
h2{
font-size: italic;
}
.style,
h2{
 background: green;
}
```

##### 混合参数

参数mixin使用一个或多个参数，通过参数和其属性来扩展Less的功能，以便在混合到另一个块时自定义mixin输出。

```
.border(@width;@style;@color){
	border: @width @style @color;
}
.myheader{
	.border(2px; dashed; green);
}
```

##### Less 父选择器

可以使用&(和号)运算符来引用父选择器。嵌套规则的父选择器由&运算符表示，并在将修改类或伪类应用于现有选择器时使用。
