##### background-size值为cover、100%和contain的区别

正如属性意义一样，三者都是来定义背景图像显示尺寸大小的，但是三者还是有一点区别的：

cover：是按照等比缩放铺满整个区域。如果显示比例和显示区域的比列相差很大某些部分会不显示，比如长度比宽度大很多，则宽度左侧会有一部分不显示。

100%：长宽100%显示，可能会拉升图片。

contain：也是等比缩放，按照某一边来覆盖显示区域的，会有白边

##### 弹性布局（display:flex;）属性详解

Flexbox是flexible box的简称（注：意思是“灵活的盒子容器”），是CSS3引入的新的布局模式。它决定了元素如何在页面上排列，使它们能在不同的屏幕尺寸和设备下可预测地展现出来。

**它之所以被称为Flexbox，是因为它能够扩展和收缩flex容器内的元素，以最大限度地填充可用空间。与以前布局方式（如table布局和浮动元素内嵌块元素）相比，Flexbox是一个更强大的方式：**

在不同方向排列元素

重新排列元素的显示顺序

更改元素的对齐方式

动态地将元素装入容器

###### 一、基本概念

采用Flex布局的元素，称为Flex容器（flex container）,简称“容器”。它的所有子元素自动称为容器成员，称为Flex项目（flex item）,简称“项目”。

在Flexbox模型中，有三个核心概念：

- flex项（注：也称flex子元素），需要布局的元素
- flex容器，其包含flex项
- 排列方向（direction）,这决定了flex项的布局方向

###### 二、容器属性

![img](https://img2018.cnblogs.com/blog/1287814/201902/1287814-20190227105554256-71254015.png)

###### flex-direction:

row（默认值）：主轴为水平方向，起点在左端

row-reverse：主轴为水平方向，起点在右端

column：主轴为垂直方向，起点在上沿

column-reverse：主轴为垂直方向，起点在下沿

![img](https://img2018.cnblogs.com/blog/1287814/201902/1287814-20190227112100971-1704943994.png)

###### flex-wrap:

nowrap(默认)： 不换行

wrap: 换行，第一行在上方

wrap-reverse: 换行，第一行在下方

###### justify-content:

flex-start(默认值)：左对齐

flex-end：右对齐

center: 居中

space-between：两端对齐，项目之间的间隔都相等。

space-around: 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

###### align-items:

flex-start：交叉轴的起点对齐

flex-end：交叉轴的终点对齐

center：交叉轴的中点对齐

baseline:项目的第一行文字的基线对齐

stretch(默认值)：如果项目未设置高度或设为auto，将沾满整个容器的高度。

###### align-content:

定义了多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用

flex-start:与交叉轴的起点对齐

flex-end：与交叉轴的终点对齐

center：与交叉轴的中点对齐

space-between：与交叉轴两端对齐，轴线之间的间隔平均分布

space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍

stretch(默认值)：轴线沾满整个交叉轴

###### 项目属性

![img](https://img2018.cnblogs.com/blog/1287814/201902/1287814-20190227135037726-1170549985.png)



###### flex-grow属性

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

###### flex-shrink属性

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。

###### align-self属性

align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

**弹性布局默认不改变项目的宽度，但是它默认改变项目的高度。如果项目没有显式指定高度，就将占据容器的所有高度。**

![img](https://img-blog.csdnimg.cn/20201127001747932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjU1NDE5MQ==,size_16,color_FFFFFF,t_70#pic_center)
