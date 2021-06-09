##### CSS_知识点

CSS指层叠样式表（Cascading Style Sheets）

把样式添加到HTML4.0中，是为了解决内容与表现分离的问题

多重样式优先级：内联样式 inline style > 内部样式 internal style sheet > 外部样式 external style sheet > 浏览器默认样式

##### CSS背景属性

```
background      简写属性，作用是将背景属性设置在一个声明中
background-attachment 背景图像是否固定或者随着页面的其余部分滚动
background-color 设置元素的背景颜色
background-image 把图像设置为背景
background-position 设置背景图像的起始位置
background-repeat   设置背景图像是否及如何重复

body {background:#ffffff url('img_tree.png') no-repeat right top;}
当使用简写属性时，属性值的顺序为：
background-color background-image background-repeat background-attachment  background-position

```

##### 文本的对齐方式

text-align: center /right / justify

##### 文本修饰 

text-decoration 属性用来设置或删除文本的装饰。

text-decoration:none/overline/line-through/underline

##### 文本转换

文本转换属性是用来指定在一个文本中的大写和小写字母。可用于所有字句变成大写或小写字母，或每个单词的首字母大写。

text-transform:uppercase/lowercase/capitalize

##### 文本缩进 

文本缩进属性是用来指定文本的第一行的缩进。

text-indent: 50px;

##### 所有CSS文本属性

```
color          设置文本颜色
direction      设置文本方向
letter-spacing 设置字符间距
line-height    设置行高
text-align     对齐元素中的文本
text-decoration 向文本添加修饰
text-indent     缩进元素中文本的首行
text-shadow     设置文本阴影
text-transform  控制元素中的字母
unicode-bidi    设置或返回文本是否被重写
vertical-align  设置元素的垂直对齐
white-space     设置元素中空白的处理方式
word-spacing    设置字间距
```

##### 所有CSS字体属性

```
font    在一个声明中设置所有的字体属性
font-family 指定文本的字体系列
font-size   指定文本的字体大小
font-style  指定文本的字体样式
font-variant 以小型大写字体或者正常字体显示文本
font-weight  指定字体的粗细
```

##### 	CSS链接

```
a:link  - 正常，未访问过的链接
a:visited - 用户已访问过的链接
a:hover  - 当用户鼠标放在链接上时
a:active - 链接被点击的那一刻
```

##### CSS列表

list-style-type属性指定列表项标记的类型

circle/square/upper-roman/lower-alpha

list-style 指 list-style-type 、list-style-position、list-style-image的简写

##### 所有的CSS列表属性

```
list-style  简写属性。用于把所有用于列表的属性设置于一个声明中
list-style-image  将图像设置为列表项标志
list-style-position 设置列表中列表项标志的位置
list-style-type     设置列表项标志的类型
```

##### 表格边框 border

border-collapse 属性设置表格的边框是否被折叠成一个单一的边框或隔开。

##### CSS盒子模型

Margin(外边距): - 清楚边框外的区域，外边距是透明的。

Border(边框): - 围绕在内边距和内容外的边框。

Padding(内边距): - 清除内容周围日的区域，内边距是透明的。

Content(内容): - 盒子的内容，显示文本和图像。

##### CSS边框

border-style 属性用来定义边框的样式

none 默认无边框 

dotted 定义一个点线边框

dashed 定义一个虚线边框

solid 定义实线边框

double 定义两个边框。两个边框的宽度和border-width的值相同

groove:定义3D沟槽边框。效果取决于边框的颜色值

ridge:定义3D脊边框。效果取决于边框的颜色值

inset:定义一个3D的嵌入边框。效果取决于边框的颜色值

outset:定义一个3D突出边框。效果取决于边框的颜色值

border : border-width border-style(required) border-color

##### CSS轮廓（outline）

轮廓是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

轮廓outline属性指定元素轮廓的样式、颜色和宽度。

outline:  outline-width outline-style outline-color

outline是不占空间的，既不会增加额外的width或者height(这样不会导致浏览器渲染时出现reflow或是repaint)

outline有可能是非矩形的（火狐浏览器下）

##### CSS分组和嵌套选择器

分组选择器： 每个选择器用逗号分隔

嵌套选择器： 它可能适用于选择器内部的选择器的样式。

##### 所有CSS尺寸(Dimension)属性

height   设置元素的高度

line-height  设置行高

max-height  设置元素的最大高度

max-width   设置元素的最大宽度

min-height    设置元素的最小高度

min-width    设置元素的最小宽度

width            设置元素的宽度

##### CSS Display(显示)与Visibility（可见性）

display属性设置一个元素如何显示，visibility属性指定一个元素应可见还是隐藏。

隐藏一个元素可以通过把display属性设置为none，或者把visibility属性设置为hidden。但请注意，这两种方法会产生不同的结果。visibility:hidden可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间。也就是说，该元素虽然被隐藏了，但仍然会影响布局。

display:none可以隐藏某个元素，且隐藏的元素不会占用任何空间。也就是说，该元素不但被隐藏了，而且该元素原本占用的空间也会从页面布局中消失。

##### CSS Position(定位)

position: static/relative/fixed/absolute/sticky

static : HTML元素的默认值，即没有定位，遵循正常的文档流对象。

静态定位的元素不会受到top、bottom、left、right影响。

fixed定位：元素的位置相对于浏览器窗口是固定位置。即使窗口是滚动的它也不会移动。

fixed定位使元素的位置与文档流无关，因此不占据空间。fixed定位的元素和其他元素重叠。

relative定位：相对定位元素的定位是相对其正常位置。

移动相对定位元素，但它原本所占的空间不会改变。

相对定位元素经常被用来作为绝对定位元素的容器块。

absolute定位：绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于<html>

absolute定位使元素的位置与文档流无关，因此不占据空间。absolute定位的元素和其他元素重叠。

sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。

**position: sticky;** 基于用户的滚动位置来定位。

粘性定位的元素是依赖于用户的滚动，在 **position:relative** 与 **position:fixed** 定位之间切换。

它的行为就像 **position:relative;** 而当页面滚动超出目标区域时，它的表现就像 **position:fixed;**，它会固定在目标位置。

元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。

这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

重叠的元素：z-index 具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面。

注意：如果两个定位元素重叠，没有指定z-index，最后定位在HTML代码中的元素将被显示在最前面。

##### CSS布局-overflow

overflow属性用于控制内容溢出元素框时显示的方式。

CSS overflow属性可以控制内容溢出元素框时在对应的元素区间内添加滚动条。

visible      默认值。内容不会被修剪，会呈现在元素框之外。

hidden     内容会被修剪，并且其余内容是不可见的。

scroll        内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。

auto         如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。

inherit      规定应该从父元素继承overflow属性的值。

##### CSS组合选择符

在CSS3中包含了四种组合方式：

后代选择器（以空格分隔） 后代选择器用于选取某元素的后代元素

子元素选择器（以大于 > 号分隔）子元素选择器只能选择作为某元素直接/一级子元素的元素

相邻兄弟选择器（以加号+分隔）可选紧紧接在另一元素后的元素，且二者有相同父元素

普通兄弟选择器（以波浪号~分隔）选取所有指定元素之后的相邻兄弟元素

##### CSS伪类

CSS伪类是用来添加一些选择器的特殊效果。

##### CSS伪元素

CSS伪元素是用来添加一些选择器的特殊效果。

##### CSS媒体类型

@media规则

@media规则允许在相同样式表为不同媒体设置不同的样式。

##### CSS属性选择器

具有特定属性的HTML元素样式

[title]{}  包含标题title的所有元素

CSS属性选择器 ~=，|=，^=，$=，*=的区别

“value是完整单词”类型的比较符号 ~=、|=

“拼接字符串”类型的比较符号 *=、^=、$=

[attribute|=value]属性中必须是完整且唯一的单词，或者以-分隔开；

例如：

```
[lang|=en] --->  <p lang='en'> <p lang='en-us'>
```

[attribute~=value]属性中包含独立的单词为value，例如：

```
[title~=flower] ---> <img src='/i/eg_tulip.jpg' title='tulip flower' />
```

[attribute*=value] 属性中做字符串拆分，只要能拆出来 value 这个词就行，例如：

```
[title*=flower]   -->  <img src="/i/eg_tulip.jpg" title="ffffflowerrrrrr" />
```

[attribute^=value] 属性的前几个字母是 value 就可以，例如：

```
[lang^=en]    -->  <p lang="ennn">
```

**attribute 属性以 value 结尾:**

```
[attribute$=value] 属性的后几个字母是 value 就可以，例如：
a[src$=".pdf"]
```

CSS3已完全向后兼容。

一些最重要CSS3模块如下：选择器、盒模型、背景和边框、文字特效、2D/3D转换、动画、多列布局、用户界面

##### CSS3圆角

border-radius 用于创建圆角

四个值：第一个值为左上脚，第二个值为右上角，第三个值为右下角，第四个值为左下脚

border-top-left-radius 定义左上角的弧度

border-top-right-radius 定义右上角的弧度

border-bottom-right-radius 定义右下角的弧度

border-bottom-left-radius 定义左下角的弧度

##### CSS3盒阴影

box-shadow 属性被用来添加阴影

CSS3边界图片

border-image 允许你指定一个图片作为边框

##### CSS3背景

background-image 添加背景图片

background-size    指定背景图像的大小

background-origin  指定了背景图像的位置区域（content-box,padding-box,border-box区域内可以防止背景图像）

background-clip 背景剪裁属性是从指定位置开始绘制

##### CSS3渐变（Gradients）

CSS3定义了两种类型的渐变(gradients):

线性渐变（linear gradients）- 向下/向上/向左/向右/对角方向

径向渐变（radial gradients）- 由它们的中心定义

repeating-linear-gradient() 函数用于重复线性渐变

repeating-radial-gradient()函数用于重复径向渐变

##### CSS3文本效果

text-shadow     用于文本阴影

box-shadow      用于盒子阴影

text-overflow 指定应向用户如何显示溢出内容

word-wrap:break-word;允许强制文本换行-即使意味着分裂它中间的一个字

word-break:break-all;单词拆分换行属性指定换行规则

##### 新本属性

```
hanging-punctuation  规定标点字符是否位于线框之外
punctuation-trim     规定是否对标点字符进行修剪
text-align-last      设置如何对齐最后一行或紧挨着强制换行符之前的行
text-emphasis        向元素的文本应用重点标记以及重点标记的前景色
text-justify         规定当text-align设置为"justify"时所使用的对齐方法
text-outline         规定文本的轮廓
text-overflow        规定当文本溢出包含元素时发生的事情
text-shadow          向文本添加阴影
text-wrap            规定文本的换行规则
word-break           规定非中日韩文本的换行规则
word-wrap            允许对长的不可分割的单词进行分割并换行到下一行
```

##### CSS3字体

CSS3 @font-face 规则

使用以前CSS的版本，网页设计师不得不使用用户计算机上已经安装的字体。

使用CSS3，网页设计师可以使用他/她喜欢的任何字体。

当你发现您要使用的字体文件时，只需简单的将字体文件包含在网站中，它会自动下载给需要的用户。

@font-face规则定义：

font-family      name     必需。规定字体的名称

src                    URL        必需。定义字体文件的URL

fon-stretch                     可选，定义如何拉伸字体

font-style    normal/italic/oblique  可选。定义字体的样式。默认是normal

font-weight                       可饿选。定义字体的粗细。默认是normal

unicode-range   unicode-range    可选。定义字体支持的UNICODE字符范围。默认是"U+0-10FFFF"

##### CSS3 2D转换

CSS3转换可以对元素进行移动、缩放、转动、拉长或拉伸。

转换的效果是让某个元素改变形状、大小和位置。

2D转换方法：

translate() 根据左(X轴)和顶部(Y轴)位置给定的参数，从当前元素位置移动。

```
transform:translate(50px,100px);从左边元素移动50个像素，并从顶部移动100像素
```

rotate() 在一个给定度数顺时针旋转的元素。负值是允许的，这样是元素逆时针旋转。

```
transform: rotate(30deg); 元素顺时针旋转30度
```

scale() 该元素增加或减少的大小，取决于宽度(X轴)和高度(Y轴)的参数

```
transform:scale(2,3) 转变宽度为原来的大小的2倍，和其原始大小3倍的高度
```

skew()

```
transform:skew(<angle>[,<angle>])
包含两个参数值，分别表示X轴和Y轴倾斜的角度，如果第二噶参数为空，则默认为0，参数为负表示向相反方向倾斜。
skewX(<angle>)：表示只在X轴（水平方向）倾斜
skewY(<angle>):表示只在Y轴（垂直方向）倾斜
skew(30deg,20deg)元素在X轴和Y轴上倾斜20度30度
```

matrix()方法和2D变换方法合并成一个。

matrix方法有六个参数，包含旋转，缩放，移动（平移）和倾斜功能。

新转换属性

transform   适用于2D或3D转换的元素

transform-origin  允许您更改转化元素的位置

##### CSS3 3D转换

rotateX() 方法，围绕其在一个给定度数X轴旋转的元素。

rotateY()方法，围绕其在一个给定度数Y轴旋转的元素。

转换属性

```
transform      向元素应用2D或3D转换
transform-origin 允许你改变被转换元素的位置
transform-style  规定被嵌套元素如何在3D空间中显示
perspective      规定3D元素的透视效果
perspective-origin 规定3D元素的底部位置
backface-visibility 定义元素在不面对屏幕时是否可见
```

##### CSS3过渡

CSS3过渡是元素从一种样式逐渐改变为另一种效果。

要实现这一点，必须规定两项内容： 1）指定要添加效果的CSS属性  2）指定效果的持续时间

注意：如果未指定的期限，transition将没有任何效果，因为默认值是0。

过渡属性

```
transition               简写属性，用于在一个属性找那个设置四个过渡属性
transition-property      规定应用过渡的CSS属性的名称
transition-duration      定义过渡效果花费的时间。默认是0
transition-timing-function 规定过渡效果的时间曲线。默认是 ease
transition-delay         规定过渡效果何时开始。默认是0
```

##### CSS3 动画

CSS3 @keyframes规则

@keyframes 规则是创建动画

@keyframes 规则内指定一个CSS样式和动画将逐步从目前的样式更改为新的样式。

当在@keyframes 创建动画，把它绑定到一个选择器，否则动画不会有任何效果。

指定至少这两个CSS3的动画属性绑定向一个选择器： 1）规定动画的名称 2）规定动画的时长

CSS3动画是什么？

动画是使元素从一种样式逐渐变化为另一种样式的效果。

可以改变任意多的样式任意多的次数。

请用百分比来规定变化发生的时间，或用关键词from 和 to ，等同于 0% to 100% 。0%是动画的开始，100%是动画的完成。

CSS3的动画属性

```
@keyframes                   规定动画
animation                    所有动画属性的简写属性
animation-name               规定@keyframes动画的名称
animation-duration           规定动画完成一个周期所花费的秒或毫秒。默认是0
animation-timing-function    规定动画的速度曲线。默认是ease
animation-fill-mode 规定当动画不播放时（当动画完成时。或当动画有一个延迟未开始播放时），要应用到元素的样式
animation-delay              规定动画何时开始。默认是0.
animation-iteration-count    规定动画被播放的次数。默认是1.
animation-direction          规定动画是否在下一周期逆向地播放。默认是normal
animation-play-state         规定动画是否正在运行或暂停。默认是running
```

##### CSS3多列

CSS3多列属性

```
column-count               指定元素应该被分割的列数
column-fill                指定如何填充列
column-gap                 指定列与列之间的间隙
column-rule                所有column-rule-*属性的简写
column-rule-color          指定两列间边框的颜色
column-rule-style          指定两列间边框的样式
column-rule-width          指定两列间边框的厚度
column-span                指定元素要跨越多少列
column-width               指定列的宽度
columns                    column-width与column-coount的简写属性
```

##### CSS3用户界面

CSS3调整尺寸（Resizing）

CSS3中，resize属性指定一个元素是否应该由用户去调整大小。

CSS3方框大小调整(Box Sizing)

box-sizing 属性允许您以确切的方式定义适应某个区域的具体内容。

CSS3外形修饰（outline-offset）

outline-offset 属性对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓。

轮廓与边框有两点不同：1）轮廓不占用空间 2）轮廓可能是非矩形

新的用户界面特性

```
appearance    允许您使一个元素的外观像一个标准的用户界面元素
box-sizing    允许你以适应区域而用某种方式定义某些元素
icon          为创作者提供了将元素设置为图标等价物的能力
nav-down      指定在何处使用箭头向下导航键时进行导航
nav-index     指定一个元素的Tab的顺序
nav-left      指定在何处使用左侧的箭头导航键进行导航
nav-right     指定在何处使用右侧的箭头导航键进行导航
nav-up        指定在何处使用箭头向上导航键进行导航
outline-offset 外轮廓修饰并绘制超出边框的边缘
resize        指定一个元素是否是由用户调整大小
```

##### CSS3框大小

CSS3 box-sizing属性可以设置width和height属性中包含了padding(内边距)和border(边框)。

默认情况下，元素的宽度与高度计算方式如下：

**width(宽) + padding(内边距) + border(边框) = 元素实际宽度**

**height(高) + padding(内边距) + border(边框) = 元素实际高度**

如果在元素上设置了box-sizing:border-box；则padding和border也包含在width和height中。

所有元素使用box-sizing是比较推荐的。

##### CSS3 弹性盒子(Flex Box)

弹性盒子是CSS3的一种新的布局模式。

CSS3弹性盒（Flexible Box或flexbox），是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。

引入弹性盒布局模型的目的是提供一种更加有效的方式来对一个容器中的子元素进行排列、对齐和分配空白空间。

弹性盒子由弹性容器和弹性子元素组成。

弹性容器通过设置display属性的值为flex或inline-flex将其定义为弹性容器。

弹性容器内包含了一个或多个弹性子元素。

注意：弹性容器外及弹性子元素内是正常渲染的。弹性盒子只定义了弹性子元素如何在弹性容器内布局。

弹性子元素通常在弹性盒子内一行显示。默认情况每个容器只有一行。

###### flex-direction 属性指定了弹性子元素在父容器中的位置。

flex-direction: row/row-reverse/column/column-reverse

row: 横向从左到右排列（左对齐），默认的排列方式

row-reverse: 反转横向排列（右对齐），从后往前排，最后一项排在最前面

column:纵向排列

column-reverse:反纵向排列，从后往前排，最后一项排在最上面。

###### justify-content 内容对齐属性应用在弹性容器上，把弹性项沿着弹性容器的主轴线（main axis）对齐。

justify-content: flex-start | flex-end |center | space-between | space-around

- flex-start：

  弹性项目向行头紧挨着填充。这个是默认值。第一个弹性项的main-start外边距边线被放置在该行的main-start边线，而后续弹性项依次平齐摆放。

- flex-end：

  弹性项目向行尾紧挨着填充。第一个弹性项的main-end外边距边线被放置在该行的main-end边线，而后续弹性项依次平齐摆放。

- center：

  弹性项目居中紧挨着填充。（如果剩余的自由空间是负的，则弹性项目将在两个方向上同时溢出）。

- space-between：

  弹性项目平均分布在该行上。如果剩余空间为负或者只有一个弹性项，则该值等同于flex-start。否则，第1个弹性项的外边距和行的main-start边线对齐，而最后1个弹性项的外边距和行的main-end边线对齐，然后剩余的弹性项分布在该行上，相邻项目的间隔相等。

- space-around：

  弹性项目平均分布在该行上，两边留有一半的间隔空间。如果剩余空间为负或者只有一个弹性项，则该值等同于center。否则，弹性项目沿该行分布，且彼此间隔相等（比如是20px），同时首尾两边和弹性容器之间留有一半的间隔（1/2*20px=10px）。

![img](https://www.runoob.com/wp-content/uploads/2016/04/2259AD60-BD56-4865-8E35-472CEABF88B2.jpg)

###### align-items 设置或检索弹性盒子元素在侧轴（纵轴）方向上的对齐方式。

align-items: flex-start | flex-end | center | baseline | stretch

- flex-start：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。
- flex-end：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。
- center：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
- baseline：如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值将参与基线对齐。
- stretch：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。

###### flex-wrap属性用于指定弹性盒子的子元素换行方式

flex-wrap : nowrap | wrap | wrap-reverse | initial | inherit

nowrap - 默认，弹性容器为单行。该情况下弹性子项可能会溢出容器

wrap - 弹性容器为多行。该情况下弹性子项溢出的部分会被放置到新行，子项内部会发生断行

wrap-reverse - 反转wrap排列

###### align-content 属性用于修改flex-wrap属性的行为。类似于align-items，但它不是设置弹性子元素的对齐，而是设置各个行的对齐。

align-content: flex-start | flex-end | center | space-between | space-around | stretch

- `stretch` - 默认。各行将会伸展以占用剩余的空间。
- `flex-start` - 各行向弹性盒容器的起始位置堆叠。
- `flex-end` - 各行向弹性盒容器的结束位置堆叠。
- `center` -各行向弹性盒容器的中间位置堆叠。
- `space-between` -各行在弹性盒容器中平均分布。
- `space-around` - 各行在弹性盒容器中平均分布，两端保留子元素与子元素之间间距大小的一半。

##### 弹性元素属性

###### 排序

order 属性设置弹性容器内弹性子元素的属性

align-self 属性用于设置弹性元素自身在侧轴（纵轴）方向上的对齐方式

align-self: auto | flex-start | flex-end | center | baseline | stretch

- auto：如果'align-self'的值为'auto'，则其计算值为元素的父元素的'align-items'值，如果其没有父元素，则计算值为'stretch'。
- flex-start：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。
- flex-end：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。
- center：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
- baseline：如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值将参与基线对齐。
- stretch：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。

flex 属性用于指定弹性子元素如何分配空间

flex : auto | initial | none | inherit | [flex-frow] | [flex-shrink] | [flex-basis]

- auto: 计算值为 1 1 auto
- initial: 计算值为 0 1 auto
- none：计算值为 0 0 auto
- inherit：从父元素继承
- [ flex-grow ]：定义弹性盒子元素的扩展比率。
- [ flex-shrink ]：定义弹性盒子元素的收缩比率。
- [ flex-basis ]：定义弹性盒子元素的默认基准值。

CSS3弹性盒子属性

```
display     指定HTML元素盒子类型
flex-direction 指定了弹性容器中子元素的排列方式
justify-content 设置弹性盒子元素在主轴（横轴）方向上的对齐方式
align-items     设置弹性盒子元素在侧轴（纵轴）方向上的对齐方式
flex-wrap       设置弹性盒子的子元素超出父容器时是否换行
align-content修改flex-wrap属性的行为，类似align-items，但不是设置子元素对齐，而是设置行对齐
flex-grow        flex-direction 和 flex-wrap的简写
order            设置弹性盒子的子元素排列顺序
align-self       在弹性子元素上使用。覆盖容器的align-items属性
flex             设置弹性盒子的子元素如何分配空间
```

##### CSS3多媒体查询

CSS3的多媒体查询继承了CSS2多媒体类型的所有思想：取代了查找设备的类型，CSS3根据设置自适应显示。媒体查询可用于检测很多事情，例如：

viewport(视窗)的宽度与高度

设备的宽度与高度

朝向（智能手机横屏、竖屏）

分辨率

viewport 是用户网页的可视区域

##### CSS函数

attr()      返回选择元素的属性值

calc()      允许计算CSS的属性值，比如动态计算长度值

cubic-bezier()  定义了一个贝塞尔曲线

hsl()       使用色相、饱和度、亮度来定义颜色

hsla()    使用色相、饱和度、亮度、透明度来定义颜色

linear-gradient()    创建一个线性渐变的图像

radial-gradient()    用径向渐变创建图像

repeating-linear-gradient()    用重复的线性渐变创建图像

repeating-radial-gradient()    用重复的径向渐变创建图像

rgb()              使用红(R)、绿(G)、蓝(B)三个颜色的叠加来生成各式各样的颜色。

rgba()            使用红(R)、绿(G)、蓝(B)、透明度(A)的叠加来生成各式各样的颜色。

var()               用于插入自定义的属性值。

