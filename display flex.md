##### display: flex

网页布局(layout)是CSS的一个重点应用。

布局的传统解决方案，基于盒状模型，依赖display属性 + position属性 + float属性。它对于那些特殊布局非常不方便，比如垂直居中就不容易实现。

2009年，W3C提出了一种新的方案----Flex布局，可以简单、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，这意味着，现在就能很安全地使用这项功能。

> Flexbox 是 flexible box 的简称（注：意思是“灵活的盒子容器”），是 CSS3 引入的新的布局模式。它决定了元素如何在页面上排列，使它们能在不同的屏幕尺寸和设备下可预测地展现出来。

**它之所以被称为 Flexbox ，是因为它能够扩展和收缩 flex 容器内的元素，以最大限度地填充可用空间**。与以前布局方式（如 table 布局和浮动元素内嵌块元素）相比，Flexbox 是一个更强大的方式：

- **在不同方向排列元素**
- **重新排列元素的显示顺序**
- **更改元素的对齐方式**
- **动态地将元素装入容器**

###### 一、Flex布局是什么？

Flex是Flexible Box的缩写，意为“弹性布局”，用来为盒装模型提供最大的灵活性。

任何一个容器都可以指定为Flex布局。

```
.box{
	display: flex;
}
```

行内元素也可以使用Flex布局。

```
.box{
	display: inline-flex;
}
```

注意，设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。

###### 二、基本概念

采用Flex布局的元素，称为Flex容器(flex container)，简称“容器”。它的所有子元素自动成为容器成员，称为Flex项目(flex item),简称“项目”。

![img](https://www.runoob.com/wp-content/uploads/2015/07/3791e575c48b3698be6a94ae1dbff79d.png)

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size,占据的交叉轴空间叫做cross size。

###### 三、容器的属性

![img](https://img2018.cnblogs.com/blog/1287814/201902/1287814-20190227105554256-71254015.png)

###### flex-direction:

- row(默认值)：主轴为水平方向，起点在左端。
- row-reverse:主轴为水平方向，起点在右端。
- column: 主轴为垂直方向，起点在上沿。
- columen-reverse:主轴为垂直方向，起点在下沿。

###### flex-wrap:

- npwrap(默认)：不换行
- wrap:换行，第一行上方
- wrap-reverse:换行，第一行下方

###### justify-content:

- flex-start(默认值)：左对齐
- flex-end:右对齐
- center:居中
- space-between:两端对齐，项目之间的间隔都相等
- space-around:每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

###### align-items:

- flex-start:交叉轴的起点对齐
- flex-end:交叉轴的终点对齐
- center:交叉轴的中点对齐
- baseline:项目的第一行文字的基线对齐
- stretch(默认值)：如果项目未设置高度或设为auto,将沾满整个容器的高度。

###### align-center:

定义了多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用

- flex-start：与交叉轴的起点对齐。
- flex-end：与交叉轴的终点对齐。
- center：与交叉轴的中点对齐。
- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
- stretch（默认值）：轴线占满整个交叉轴。

###### 四、项目属性

![img](https://img2018.cnblogs.com/blog/1287814/201902/1287814-20190227135037726-1170549985.png)

###### order属性

![img](https://img2020.cnblogs.com/blog/1287814/202008/1287814-20200814180820136-1259976518.png)

###### flex-grow属性

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

![img](https://img2020.cnblogs.com/blog/1287814/202008/1287814-20200814180912736-504097544.png)

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

```
.item{
	flex-shrink: <number>; /* default 1 */
}
```

如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

负值对该属性无效。

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071015.jpg)

###### align-self属性

align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

```
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

**弹性布局默认不改变项目的宽度，但是它默认改变项目的高度。如果项目没有显式指定高度，就将占据容器的所有高度。**
