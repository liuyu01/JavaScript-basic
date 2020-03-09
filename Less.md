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

