##### 流

**“流”又叫文档流，是css的一种基本定位和布局机制**。流是html的一种抽象概念，暗喻这种排列布局方式好像水流一样自然自动。“流体布局”是html默认的布局机制，如你写的html不用css，默认自上而下（块级元素如`div`）从左到右（内联元素如`span`）堆砌的布局方式。

##### 块级元素和内联元素

块级元素是指单独撑满一行的元素，如`div、ul、li、table、p、h1`等元素。这些元素的display值默认是`block、table、list-item`等。

内联元素又叫行内元素，指只占据它对应标签的边框所包含的空间的元素，这些元素如果父元素宽度足够则并排在一行显示的，如`span、a、em、i、img、td`等。这些元素的display值默认是`inline、inline-block、inline-table、table-cell`等。

实际开发中，我们经常把`display`计算值为`inline` `inline-block` `inline-table` `table-cell`的元素叫做内联元素，而把`display`计算值为`block`的元素叫做块级元素。

##### css权重和超越`!important`

// 假设下面样式都作用于同一个节点元素`span`，判断下面哪个样式会生效
body#god div.dad span.son {width: 200px;}
body#god span#test {width: 250px;}

css选择器权重列表如下：

![1565316737725](C:\Users\e-Yu.Liu\AppData\Roaming\Typora\typora-user-images\1565316737725.png)

当两个权值进行比较的时候，是从高到低逐级将等级位上的权重值（如 权值 1,0,0,0、0,1,0,0、0,0,1,0、0,0,0,1 对应--> 第一等级权重值，第二等级权重值，第三等级权重值，第四等级权重值）来进行比较的，而不是简单的 1000*个数 + 100*个数 + 10*个数 + 1*个数 的总和来进行比较的，换句话说，低等级的选择器，个数再多（只要不超过256个，因为css权重256进制，我打赌世界上没有一个样式会写256个class的）也不会越等级超过高等级的选择器的优先级的。

正确规则：

1. 先从高等级进行比较，高等级相同时，再比较低等级的，以此类推；
2. 完全相同的话，就采用 后者优先 原则；

在css中，`!important`的权重相当的高。如果出现了`!important`，则不管权重如何都取有`!important`的属性值。但是宽高有例外情况，由于宽高会被`max-width`/`min-width`覆盖，所以`!important`会失效。

##### 盒模型（盒尺寸）

元素的内在盒子是由`margin box`、`border box`、`padding box`、`content box`组成的，这四个盒子由外到内构成了盒模型。

IE模型： `box-sizing: border-box`  此模式下，元素的宽度计算为`border+padding+content`的宽度总和。

w3c标准模型： `box-sizing: content-box` 此模式下，元素的宽度计算为`content`的宽度。

由于`content-box`在计算宽度的时候不包含`border pading`很烦人，而且又是默认值，业内一般采用以下代码重置样式：

:root {
box-sizing: border-box;    
}

*{
box-sizing: inherit;
}

`padding`不可为负值，但是可以为百分比值。为百分比时水平和垂直方向的`padding`都是相对于父级元素宽度计算的。

作为外边距，`margin`属性并不会参与盒子宽度的计算，但通过设置`margin`为负值，却能改变元素水平方向的尺寸

块级元素的垂直方向会发生`margin`合并，存在以下三种场景：

- 相邻兄弟元素之间`margin`合并；
- 父元素`margin-top`和子元素`margin-top`，父元素`margin-bottom`和子元素`margin-bottom`；
- 空块级元素自身的`margin-top`和`margin-botom`合并，例子如下。

要阻止`margin`合并，可以：1. 把元素放到`bfc`中；2. 设置`border`或`padding`阻隔`margin`；3.  用内联元素（如文字）阻隔；4. 给父元素设定高度。

margin`的百分比值跟`padding`一样，垂直方向的`margin`和水平方向上的一样都是相对于父元素宽度计算的。

##### `display: none`与`visibility: hidden`的区别

1. `display: none`的元素不占据任何空间，`visibility: hidden`的元素空间保留；
2. `display: none`会影响css3的`transition`过渡效果，`visibility: hidden`不会；
3. `display: none`隐藏产生重绘 ( repaint ) 和回流 ( relfow )，`visibility: hidden`只会触发重绘；（回流重绘知识点参考：[真正理解重绘和回流](https://link.juejin.im?target=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000018181862)）
4. 株连性：`display: none`的节点和子孙节点元素全都不可见，`visibility: hidden`的节点的子孙节点元素可以设置 `visibility: visible`显示。`visibility: hidden`属性值具有继承性，所以子孙元素默认继承了`hidden`而隐藏，但是当子孙元素重置为`visibility: visible`就不会被隐藏。

`text-align: justify`为两端对齐。

网格布局（Grid）是最强大的 CSS 布局方案。注意，设为网格布局以后，容器子元素（项目）的`float、display: inline-block、display: table-cell、vertical-align和column-*等`设置都将失效

弹性布局是指`display: flex`或`display: inline-flex`的布局。注意，设为弹性布局以后，子元素的`float、clear、vertical-align`属性都会失效。

