##### 什么是 CSS?

CSS 指层叠样式表 (Cascading Style Sheets)

样式定义如何显示 HTML 元素

样式通常存储在样式表中

把样式添加到 HTML 4.0 中，是为了解决内容与表现分离的问题

外部样式表可以极大提高工作效率

外部样式表通常存储在 CSS 文件中

多个样式定义可层叠为一个

##### CSS语法

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明

![1581822970627](C:\Users\e-Yu.Liu\AppData\Roaming\Typora\typora-user-images\1581822970627.png)

##### CSS注释

CSS注释以 "/*" 开始, 以 "*/" 结束

##### CSS Id和Class 

CSS 中 id 选择器以 "#"来定义。

在 CSS 中，类选择器以一个点"."号显示

##### CSS 创建

###### 如何插入样式表

插入样式表的方法有三种:

外部样式表(External style sheet)

内部样式表(Internal style sheet)

内联样式(Inline style)

###### 外部样式表

当样式需要应用于很多页面时，外部样式表将是理想的选择。在使用外部样式表的情况下，你可以通过改变一个文件来改变整个站点的外观。每个页面使用 <link> 标签链接到样式表。 <link> 标签在（文档的）头部

```
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

###### 内部样式表

当单个文档需要特殊的样式时，就应该使用内部样式表。你可以使用 <style> 标签在文档头部定义内部样式表

```
<head>
<style>
hr {color:sienna;}
p {margin-left:20px;}
body {background-image:url("images/back40.gif");}
</style>
</head>
```

###### 内联样式

由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势。请慎用这种方法，例如当样式仅需要在一个元素上应用一次时。

要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style 属性可以包含任何 CSS 属性。

```
<p style="color:sienna;margin-left:20px">这是一个段落。</p>
```

###### 样式优先级

（内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式

##### CSS背景

background     简写属性，作用是将背景属性设置在一个声明中

background-attachment 背景图像是否固定或者随着页面的其余部分滚动

background-color   设置元素的背景颜色

background-image   把图像设置为背景

background-position 设置背景图像的起始位置

background-repeat    设置背景图像是否及如何重复

当使用简写属性时，属性值的顺序为：:background-color
、background-image、background-repeat、background-attachment、background-position

##### CSS文本格式

color     设置文本颜色

direction  设置文本方向

letter-spacing 设置字符间距

line-height  设置行高

text-align 对齐元素中的文本

text-decoration 向文本添加修饰

text-indent 缩进元素中文本的首行

text-shadow 设置文本阴影

text-transform 控制元素中的字母

unicode-bidi  设置或返回文本是否被重写

vertical-align   设置元素的垂直对齐

white-spacing 设置元素中空白的处理方式

word-spacing  设置字间距

letter-spacing 这个样式使用在英文单词时，是设置字母与字母之间的间距。如果想设置英文单词之间的间距，可以使用 word-spacing 来实现。

汉字的间隔调节也是用 letter-spacing 来实现的。因为中文段落里字与字之间没有空格，因而 word-spacing 通常起不到调整间距的作用。

##### CSS字体

font      在一个声明中设置所有的字体属性

font-family  指定文本的字体系列

fot-size   指定文本的字体大小

font-sytle  指定文本的字体样式

font-variant  以小型大写字体或者正常字体显示文本

font-weight  指定字体的粗细

如果你不指定一个字体的大小，默认大小和普通文本段落一样，是16像素（16px=1em）。

###### 用em来设置字体大小

为了避免Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。

em的尺寸单位由W3C建议。

1em和当前字体大小相等。在浏览器中默认的文字大小是16px。

因此，1em的默认大小是16px。可以通过下面这个公式将像素转换为em：px/16=em

##### CSS链接

a:link  - 正常，未访问过的链接

a:visited -  用户已访问过的链接

a:hover - 当用户鼠标放在链接上时

a:active  - 链接被点击的那一刻

css定义超链接是要有先后顺序的。否则，在某些浏览器里面有可能会出现某个样式不起作用的bug。不能正确显示想要的效果。CSS属性的排列顺序: L-V-H-A 。 L-V-H-A是link、visited、hover、active的简写。   

##### CSS列表

list-style  简写属性。用于把所有用于列表的属性设置于一个声明中

list-style-image  将图像设置为列表项标志

list-style-position  设置列表中列表项标志的位置

list-style-type 设置列表项标志的类型

简写属性可以按顺序设置如下属性：list-style-type、list-style-position、list-style-image

##### CSS表格

border  表格边框

border-collapse 设置表格的边框是否被折叠成一个单一的边框或隔开

height 和 width属性定义表格的宽度和高度

text-align属性设置水平对齐方式，向左，右，或中心

vertical-align属性设置垂直对齐方式

##### CSS盒子模型

css盒子模型（Box Model） 所有HTML元素可以看作盒子，在css中，‘box model’这一术语是用来设计和布局时使用。CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充和实际内容。

盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。

![1581836128192](C:\Users\e-Yu.Liu\AppData\Roaming\Typora\typora-user-images\1581836128192.png)

margin(外边距)：清除边框外的区域，外边距是透明的

border(边框)：围绕在内边距和内容外的边框

padding(内边距)：清除内容周围的区域，内边距是透明的

content(内容)：盒子的内容，显示文本和图像

最终元素的总宽度计算公式是这样的：

总元素的宽度=宽度+左填充+右填充+左边框+右边框+左边距+右边距

总元素的高度=高度+顶部填充+底部填充+上边框+下边框+上边距+下边距

###### W3C标准盒子模型

![1581836587682](C:\Users\e-Yu.Liu\AppData\Roaming\Typora\typora-user-images\1581836587682.png)

从上图可以看出，w3c盒子模型的范围包括margin、border、padding、content,并且content部分不包含其他部分。

###### IE盒子模型

![1581836664194](C:\Users\e-Yu.Liu\AppData\Roaming\Typora\typora-user-images\1581836664194.png)

从上图可以看出，IE盒子模型的范围包括margin、border、padding、content,和w3c盒子模型不同的是，IE盒子模型的content部分包含了padding和border.

IE5.5及更早的版本使用的是IE盒模型。IE6及其以上的版本在标准兼容模式下使用的是W3C的盒模型标准。我们说这是一个好消息因为这意味着此盒模型问题

只会出现在IE5.5及其更早的版本中。只要为文档设置一个DOCTYPE，就会使得IE遵循标准兼容模式的方式工作。

另外，我们现在应该能理解了，css3的box-sizing属性给了开发者选择盒模型解析方式的权利。W3C的盒模型方式被称为“content-box”，IE的被称为“border-box”，使用box-sizing: border-box;就是为了在设置有padding值和border值的时候不把宽度撑开。

##### CSS边框

border    简写属性，用于把针对四个边的属性设置在一个声明

border-style  用于设置元素所有边框的样式，或者单独地为各边设置边框样式

border-width 简写属性，用于为元素的所有边框设置宽度，或者单独地为各边边框设置宽度

border-color 简写属性，设置元素的所有边框中可见部分的颜色，或为4个边分别设置颜色

border-bottom 简写属性，用于把下边框的所有属性设置到一个声明中。

border-bottom-color  设置元素的下边框的颜色。

border-bottom-style  设置元素的下边框的样式。

border-bottom-width  设置元素的下边框的宽度。

border-left   简写属性，用于把左边框的所有属性设置到一个声明中。

border-left-color 设置元素的左边框的颜色。

border-left-style 设置元素的左边框的样式。

border-left-width 设置元素的左边框的宽度。

border-right  简写属性，用于把右边框的所有属性设置到一个声明中。

border-right-color   设置元素的右边框的颜色。

border-right-style   设置元素的右边框的样式。

border-right-width   设置元素的右边框的宽度。

border-top 简写属性，用于把上边框的所有属性设置到一个声明中。

border-top-color  设置元素的上边框的颜色。

border-top-style  设置元素的上边框的样式。

border-top-width  设置元素的上边框的宽度。

border-style:属性1，属性2，属性3，属性4    上-》右-》下-》左

border-style:属性1，属性2，属性3      上-》左右-》下

border-style：属性1，属性2， 上下-》左右

border-style:属性1   上下左右相同

##### CSS轮廓（outline）

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

轮廓（outline）属性指定元素轮廓的样式、颜色和宽度。

outline   在一个声明中设置所有的轮廓属性

outline-color 设置轮廓的颜色

outline-style  设置轮廓的样式

outline-width 设置轮廓的宽度

outline是不占空间的，既不会增加额外的width或者height(这样不会导致浏览器渲染时出现reflow或是repaint)

##### CSS margin(外边距)

margin  简写属性。在一个声明中设置所有外边距属性

margin-bottom  设置元素的下外边距

margin-left  设置元素的左外边距

margin-right 设置元素的右外边距

margin-top  设置元素的上外边距

margin的值可能是：auto(依赖于浏览器)、length（定义一个固定的margin）、%（定义一个使用百分比的边距）

##### CSS padding(填充)

padding   使用简写属性设置在一个声明中的所有填充属性

padding-bottom 设置元素的底部填充

padding-left  设置元素的左部填充

padding-right 设置元素的右部填充

padding-top 设置元素的顶部填充

paddiing的值可能是：length（定义一个固定的填充）、%（使用百分比值定义一个填充）

##### CSS分组和嵌套选择器

分组选择器 ： 每个选择器用逗号分隔

嵌套选择器：可能适用于选择器内部的选择器的样式

p{} :为所有p元素指定一个样式

.marked{}：为所有class='marked'的元素指定一个样式

.marked p{}：为所有class='marked'元素内的p元素指定一个样式

p .marked{}：为所有class='marked'的p元素指定一个样式

##### CSS尺寸（Dimension）

height 设置元素的高度

line-height 设置行高

max-height 设置元素的最大高度

max-width 设置元素的最大宽度

ming-height 设置元素的最小高度

min-width 设置元素的最小宽度

width 设置元素的宽度

##### CSS Display(显示)与Visibility(可见性)

display属性设置一个元素应如何显示，visibility属性指定一个元素应可见还是隐藏

###### 隐藏元素 - display:none 或 visibility:hidden

visibility:hidden 可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间。也就是说，该元素虽然被隐藏了，但仍然会影响布局。

display:none 可以隐藏某个元素，且隐藏的元素不会占用任何空间。也就是说，该元素不但被隐藏了，而且该元素原本占用的空间也会从页面布局中消失。

display:block  - 显示为块级元素

display:inline  - 显示为内联元素

display:inlne-block - 显示为内联块元素，表现为同行显示并可修改宽高内外边距等属性

块级元素（block）特性：1）总是独占一行，表现为另起一行开始，而且其后的元素也必须另起一行2）显示宽度（width)、高度（height）、内边距（padding）和外边距（margin）都可控制

内联元素（inline）特性:1)和相邻的内联元素在同一行 2）宽度（width）、高度（height）、内边距（padding）和外边距（margin）都不可改变，就是里面文字或图片的大小

##### CSS position(定位)

positon 属性的5个值 ：  static 、relative、fixed 、absolute 、sticky

###### static 定位 

HTML元素的默认值，即没有定位，遵循正常的文档流对象。静态定位的元素不会受到top、bottom、left 、right影响。

###### fixed定位

元素的位置相对于浏览器窗口是固定位置。即使窗口是滚动的它也不会移动。

fixed定位使元素的位置与文档流无关，因此不占据空间。fixed定位的元素和其他元素重叠。

###### relative定位

相对定位元素的定位是相对其正常位置。移动相对定位元素，但它原本所占的空间不会改变。相对定位元素经常被用来作为绝对定位元素的容器快。

###### absolute定位

绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于<html>。absolute 定位使元素的位置与文档流无关，因此不占据空间。absolute定位的元素和其他元素重叠。

###### sticky定位

sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。

position：sticky,基于用户的滚动位置来定位。

粘性定位的元素是依赖于用户的滚动，在position:relative 与 position:fixed定位之间切换。它的行为就像position:relative;而当页面滚动超出目标区域时，它的表现就像position:fixed;它会固定在目标位置。元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

###### 重叠的元素

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素。z-index属性指定了一个元素的堆叠顺序。一个元素可以有正数或负数的堆叠顺序。具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面。

**注意：** 如果两个定位元素重叠，没有指定z - index，最后定位在HTML代码中的元素将被显示在最前面。

##### CSS布局 - overflow

oveflow属性用于控制内容溢出元素框时显示的方式。

overflow属性有以下值：

visible   默认值。内容不会被修剪，会呈现在元素框之外

hidden  内容会被修剪，并且其余内容是不可见的

scroll      内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容

auto      如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容

inherit   规定应该从父元素继承overflow属性的值

##### CSS float 浮动

css 的float 会使元素向左或向右移动，其周围的元素也会重新排列。

元素的水平方向浮动，意味着元素只能左右移动而不能上下移动。

一个浮动元素会尽量向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。

浮动元素之后的元素将围绕它。

浮动元素之前的元素将不会受到影响。

清除浮动--使用clear

clear  指定不允许元素周围有浮动元素   left、right、both、none、inherit

float 指定一个元素是否可以浮动             left、right、none、inherit

##### CSS布局-水平&垂直对齐

元素居中对齐  margin:auto;

文本在元素内居中对齐，使用tetx-align:center;

图片居中对齐  margin:auto;

左右对齐，--使用定位方式

左右对齐，--使用float方式

垂直居中对齐--使用padding

垂直居中--使用line-height

垂直居中--使用position和transform

##### CSS组合选择符

在CSS3中包含了四种组合方式：

- 后代选择器（以空格分隔）
- 子元素选择器（以大于号分隔）
- 相邻兄弟选择器（以加号分隔） 相邻兄弟选择器可选择紧接在另一元素后的元素，且二者有相同父元素
- 后续兄弟选择器（以破折号分隔）后续兄弟选择器选取所有指定元素之后的相邻兄弟元素

##### CSS伪类

css伪类是用来添加一些选择器的特殊效果。

###### anchor伪类 

在支持css的浏览器中，链接的不同状态都可以以不同的方式显示

###### :first-child伪类

:first-child伪类来选择父元素的第一个子元素

###### :lang伪类

:lang伪类使你有能力为不同的语言定义特殊的规则

##### CSS伪元素

css伪元素是用来添加一些选择器的特殊效果

###### :first-line 伪元素

first-line伪元素用于向文本的首行设置特殊样式

###### :first-letter 伪元素

first-letter伪元素用于向文本的首字母设置特殊样式

###### :before伪元素

before伪元素可以在元素的内容前面插入新内容

###### :after伪元素

after伪元素可以在元素的内容之后插入新内容

##### CSS图像拼合技术

图像拼合就是单个图像的集合。

有许多图像的网页可能需要很长的时间来加载和生成多个服务器的请求。使用图像拼合会降低服务器的请求数量，并节省带宽。

##### CSS媒体类型

媒体类型允许你指定文件将如何在不同媒体呈现。该文件可以以不同的方式显示在屏幕上，在纸张上。或听觉浏览器等等。

一些css属性只设计了某些媒体。例如voice-family属性是专为听觉用户代理。其他一些属性可用于不同的媒体类型。例如，font-size属性可用于屏幕和印刷媒体，但有不同的值。屏幕和纸上的文件不同，通常需要一个更大的字体，sans-serif字体比较适合子屏幕上阅读，而serif字体更容易在纸上阅读。

###### @media规则

@media规则允许在相同样式表为不同媒体设置不同的样式。

all    用于所有的媒体设备

print  用于打印机

screen 用于电脑显示器

##### CSS属性选择器

具有特定属性的HTML元素样式不仅仅是class和id。

属性选择器 [title]{color:blue}

属性和值选择器[title=runoob]{border:5px solid green;}

属性和值的选择器-多值  [title~=hello]{color:blue;}

CSS属性选择器 *=，|=，^=，$=，的区别

"value 是完整单词" 类型的比较符号: ~=, |=

"拼接字符串" 类型的比较符号: *=, ^=, $=

[attribute~=value] 属性中包含独立的单词为 value，

[attribute|=value] 属性中必须是完整且唯一的单词，或者以 - 分隔开

[attribute$=value] attribute 属性以 value 结尾:

[attribute^=value]attribute 属性以 value 开头:

##### CSS计数器

css计数器通过一个变量来设置，根据规则递增变量。

css计数器使用到一下几个属性：

counter-reset  - 创建或者重置计数器

counter-increment - 递增变量

content - 插入生成的内容

counter()或counters()函数-将计数器的值添加到元素

CSS3是最新的CSS标准。

##### CSS3边框

边框属性

border-radius 用于创建圆角

box-shadow  添加阴影

border-image  设置所有边框图像

##### CSS3圆角

使用CSS3 border-radius属性，可以给任何元素制作‘圆角’。

border-radius 所有四个边角属性的缩写

border-top-left-radius  定义了左上角的弧度

border-top-right-radius  定义了右上角的弧度

border-bottom-right-radius 定义了右下角的弧度

border-bottom-left-radius 定义了左下角的弧度

如果你要在四个角上一一指定，可以使用以下规则：

四个值：第一个值为左上角，第二个值为右上角，第三个值为右下角，第四个值为左下角

三个值：第一个值为左上角，第二个值为右上角和左下角，第三个值为右下角

两个值：第一个值为左上角与右下角，第二个值为右上角与左下角

一个值：四个圆角值相同

##### CSS3背景

background-image    添加背景图片

background-size    指定背景图像的大小

background-origin 指定了背景图像的位置区域。content-box,padding-box,border-box区域内可以放置背景图像

background-clip 背景剪裁属性是从指定位置开始绘制

##### CSS3渐变

CSS3渐变可以让你在两个或多个指定的颜色之间显示平稳的过渡。

CSS3定义了两种类型的渐变：

线性渐变（Linear Gradients）-向下/向上/向左/向右/对角方向

径向渐变（Radial Gradients）-由它们的中心定义

###### 使用透明度（transparent）  

CSS3渐变也支持透明度（transparent），可用于创建减弱变淡的效果。

为了添加透明度，我们使用 rgba() 函数来定义颜色结点。rgba() 函数中的最后一个参数可以是从 0 到 1 的值，它定义了颜色的透明度：0 表示完全透明，1 表示完全不透明。

重复的线性渐变  repeating-linear-gradient()

重复的径向渐变 repeating-radial-gradient()

##### CSS3文本效果

hanging-punctuation   规定标点字符是否位于线框之外

punctuation-trim    规定是否对标点字符进行修剪

text-align-last    设置如何对齐最后一行或紧挨着强制换行符之前的行

text-emphasis   向元素的文本应用重点标记以及重点标记的前景色

text-justify   规定当text-align设置为‘justify’时所使用的对齐方法

text-outline  规定文本的轮廓

 text-overflow   规定当文本溢出包含元素时发生的事情

text-shadow     向文本添加阴影

text-wrap          规定文本的换行规则

word-break     规定非中日韩文本的换行规则

word-wrap       允许对长的不可分割的单词进行分割并换行到下一行

##### CSS3字体

CSS3 @font-face规则

使用CSS3，网页设计师可以使用他/她喜欢的任何字体。

当你发现您要使用的字体文件时，只需简单的将字体文件包含在网站中，它会自动下载给需要的用户。

##### CSS3 2D转换

CSS3转换可以对元素进行移动、缩放、转动、拉长或拉伸。

转换的效果是让某个元素改变形状，大小和位置。

transform   适用于2D或3D转换的元素

transform-origin  允许您更改转化元素位置

matrix(n,n,n,n,n,n)   定义2D转换，使用六个值的矩阵

translate(x,y) 定义2D转换，沿着X和Y轴移动元素

translateX(n)  定义2D转换，沿着X轴移动元素

translateY(n)  定义2D转换，沿着Y轴移动元素

scale(x,y)  定义2D缩放转换，改变元素的宽度和高度

scaleX(n)   定义2D缩放转换，改变元素的宽度

scaleY(n)  定义2D缩放转换，改变元素的高度

rotate(angle)  定义2D旋转，在参数中规定角度

skew(x-angle,y-angle)  定义2D倾斜转换，沿着X和Y轴

skewX(angle)   定义2D倾斜转换，沿着X轴

skewY(angle)   定义2D倾斜转换，沿着Y轴

##### CSS3 3D转换

transform  向元素应用 2D 或 3D 转换。  

transform-origin  允许你改变被转换元素的位置。    

transform-style   规定被嵌套元素如何在 3D 空间中显示。   

perspective   规定 3D 元素的透视效果。 

perspective-origin   规定 3D 元素的底部位置。 

backface-visibility  定义元素在不面对屏幕时是否可见。

matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n) 定义 3D 转换，使用 16 个值的 4x4 矩阵。

translate3d(x,y,z)   定义 3D 转化。

translateX(x) 定义 3D 转化，仅使用用于 X 轴的值。

translateY(y) 定义 3D 转化，仅使用用于 Y 轴的值。

translateZ(z) 定义 3D 转化，仅使用用于 Z 轴的值。

scale3d(x,y,z)    定义 3D 缩放转换。

scaleX(x)  定义 3D 缩放转换，通过给定一个 X 轴的值。

scaleY(y)  定义 3D 缩放转换，通过给定一个 Y 轴的值。

scaleZ(z)  定义 3D 缩放转换，通过给定一个 Z 轴的值。

rotate3d(x,y,z,angle)    定义 3D 旋转。

rotateX(angle)    定义沿 X 轴的 3D 旋转。

rotateY(angle)    定义沿 Y 轴的 3D 旋转。

rotateZ(angle)    定义沿 Z 轴的 3D 旋转。

perspective(n)    定义 3D 转换元素的透视视图。

##### CSS3过渡

css3过渡是元素从一种样式逐渐改变为另一种的效果。

要实现这一点，必须规定两项内容： 1）指定要添加效果的CSS属性  2）指定效果的持续时间

transition     简写属性，用于在一个属性中设置四个过渡属性

transition-property   规定应用过渡的CSS属性的名称

transition-duration   定义过渡效果花费的时间。默认是0

transition-timing-function  规定过渡效果甚微时间曲线。默认是ease

transition-delay  规定过渡效果何时开始。默认是0

##### CSS3动画

css3可以创建动画，它可以取代许多网页动画图像、Flash动画和JavaScript实现的效果。

###### CSS3 @keyframes规则

@keyframes规则是创建动画。

@keyframes规则内指定一个CSS样式和动画将逐步从目前的样式更改为新的样式。

当在@keyframes创建动画，把它绑定到一个选择器，否则动画不会有任何效果。

指定至少这两个CSS3的动画属性绑定向一个选择器：

- 规定动画的名称
- 规定动画的时长

##### CSS3动画是什么？

动画是使元素从一种样式逐渐变化为另一种样式的效果。

可以改变任意多的样式任意多的次数。

请用百分比来规定变化发生的时间，或用关键词from和to,等同于0%和100%

0%是动画的开始，100%是动画的完成

为了得到最佳的浏览器支持，您应该始终定义0%和100%选择器。

@keyframes                 规定动画

animation                     所有动画属性的简写属性，除了animation-play-state属性

animation-name          规定@keyframes动画的名称

animation-duration      规定动画完成一个周期所花费的秒或毫秒。默认是0

animation-timing-function   规定动画的速度曲线。默认是‘ease’

animation-fill-mode      规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式

animation-delay           规定动画何时开始。默然是0

animation-iteration-count  规定动画被播放的次数。默认是1

animation-direction      规定动画是否在下一周期逆向地播放。默认是normal

animation-play-state    规定动画是否正在运行或暂停。默认是running

##### CSS3多列

column-count     指定元素应该被分割的列数

column-fill         指定如何填充列

column-gap      指定列与列之间的间隙

column-rule     所有column-rule-*属性的简写

column-rule-color   指定两列间边框的颜色

column-rule-style  指定两列间边框的样式

column-rule-width  指定两列间边框的厚度

column-span  指定元素要跨越多少列

column-width  指定列的宽度

columns   设置column-width和column-count的简写

##### CSS3用户界面

在CSS3中，增加了一些新的用户界面特性来调整元素尺寸，框尺寸和外边框。

appearance    允许您使一个元素的外观像一个标准的用户界面元素

box-sizing       允许你以适应区域而用某种方式定义某些元素

icon     为创作者提供可将元素设置为图标等价物的能力

nav-down   指定在何处使用箭头向下导航键时进行导航

nav-index   指定一个元素的Tab的顺序

nav-left        指定在何处使用左侧的箭头导航键进行导航

nav-right      指定在何处使用右侧的箭头导航键进行导航

nav-up        指定在何处使用箭头向上导航键时进行导航

outline-offset  外轮廓修饰并绘制超出边框的边缘

resize         指定一个元素是否是由用户调整大小

CSS3 box-sizing 属性可以设置width和height属性中包含了padding(内边距)和border(边框)。

##### CSS3 多媒体查询

CSS3 的多媒体查询继承了 CSS2 多媒体类型的所有思想： 取代了查找设备的类型，CSS3 根据设置自适应显示。

媒体查询可用于检测很多事情，例如：

- viewport(视窗) 的宽度与高度
- 设备的宽度与高度
- 朝向 (智能手机横屏，竖屏) 。
- 分辨率

##### 多媒体查询语法

多媒体查询由多种媒体组成，可以包含一个或多个表达式，表达式根据条件是否成立返回 true 或 false。

```
@media not|only mediatype and (expressions) {
    CSS 代码...;
}
```

如果指定的多媒体类型匹配设备类型则查询结果返回 true，文档会在匹配的设备上显示指定样式效果。

除非你使用了 not 或 only 操作符，否则所有的样式会适应在所有设备上显示效果。

- **not:** not是用来排除掉某些特定的设备的，比如 @media not print（非打印设备）。
- **only:** 用来定某种特别的媒体类型。对于支持Media Queries的移动设备来说，如果存在only关键字，移动设备的Web浏览器会忽略only关键字并直接根据后面的表达式应用样式文件。对于不支持Media Queries的设备但能够读取Media Type类型的Web浏览器，遇到only关键字时会忽略这个样式文件。
- **all:** 所有设备，这个应该经常看到。

###### CSS3多媒体类型

all     用于所有媒体类型设备

print  用于打印机

screen  用于电脑屏幕，平板，智能手机等

speech  用于屏幕阅读器

##### CSS3弹性盒子（Flex Box）

弹性盒子是CSS3的一种新的布局模式。

CSS3弹性盒子(Flexible Box 或flexbox),是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。

引入弹性盒布局模型的目的是提供一种更加有效的方式来对一个容器中的子元素进行排列、赌气和分配空白空间。

###### CSS3弹性盒子内容

弹性盒子由弹性容器（Flex container）和弹性子元素（Flex item）组成。

弹性容器通过设置display属性的值为flex或inline-flex将其定义为弹性容器。

弹性容器内包含了一个或多个弹性子元素。

注意：弹性容器外及弹性子元素内是正常渲染的。弹性盒子只定义了弹性子元素如何在弹性容器内布局。

弹性子元素通常在弹性盒子内一行显示。默认情况每个容器只有一行。

flex-direction 指定了弹性子元素在父容器中的位置

justify-content(内容对齐)属性应用在弹性容器上，把弹性项沿着弹性容器的主轴线（main axis）对齐。

align-items设置或检索弹性盒子元素在侧轴（纵轴）方向上的对齐方式。

flex-wrap 属性用于指定弹性盒子的子元素换行方式。

`align-content` 属性用于修改 `flex-wrap` 属性的行为。类似于 `align-items`, 但它不是设置弹性子元素的对齐，而是设置各个行的对齐。

`align-self` 属性用于设置弹性元素自身在侧轴（纵轴）方向上的对齐方式。

`flex` 属性用于指定弹性子元素如何分配空间。

order 设置弹性盒子的子元素排列顺序
