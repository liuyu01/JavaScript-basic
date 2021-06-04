1.doctype 声明是不区分大小写的，用来告知Web浏览器页面使用了哪种HTML版本。

在HTML4.01中，<!DOCTYPE>声明需引用DTD(文档类型声明)，因为HTML4.01是基于SGML（Standard Generalized Markup Language标准通用标记语言）。

HTML4.01规定了三种不同的<!DOCTYPE>声明，分别是：Strict、Transitional和Frameset。

HTML5不是基于SGML，因此不要求引用DTD。

2.对于中文网页需要使用<meta charset="utf-8">声明编码，否则会出现乱码。有些浏览器（如360浏览器）会设置GBK为默认编码，则你需要设置为<meta charset="gbk">。

目前在大部分浏览器中，直接输出中文会出现中文乱码的情况，这时候需要在头部将字符声明为UTF-8。

3.当保存HTML文件时，既可以使用.htm也可以使用.html扩展名。两者没有区别，建议统一用.html。

4.HTML中href和src有什么区别？

href是Hypertext Reference 的缩写，表示超文本引用。用来建立当前元素和文档之间的链接。常用的有：link、a。例如：

```
<link href="reset.css" rel='stylesheet' />
```

浏览器会识别该文档为css文档，并行下载该文档，并且不会停止对当前文档的处理。这也是建议使用link，而不采用@import加载css的原因。src是source的缩写，src的内容是页面必不可少的一部分，是引入。src指向的内容会嵌入到文档中当前标签所在的位置。常用的有:img 、script 、iframe。例如：

```
<script src='script.js'></script>
```

当浏览器解析到该元素时，会暂停浏览器的渲染，直到该资源加载完毕。这也是将js脚本放在底部而不是头部的原因。

简而言之，src用于替换当前元素；href用于在当前文档和引用资源之间建立联系。

5.html相对路径的写法：

./ ：代表文件所在的目录（可以省略不写）如果写成image/background就相当于是在html文件下找image文件夹，当然是找不到的

../ ：代表文件所在的父级目录

../../ ：代表文件所在的父级目录的父级目录

/ : 代表文件所在的根目录

6.没有内容的HTML元素被称为空元素。空元素在开始标签中进行关闭（以开始标签的结束而结束）

7.一些标签的使用，切记所有标签都需要闭合，不管是单体标签还是成对标签。

8.标签写法要用小写字母。

9.属性和属性值，尽量小写

10.class 属性可以多用class=" "(引号里面可以填入多个class属性)

​	id属性只能单独设置id=" "(只能填写一个，多个无效)

11.当把鼠标指针移动到网页中的某个链接上时，箭头会变为一只小手。

12.HTML <meta>元素

meta标签描述了一些基本的元数据。

meta标签提供了元数据。元数据也不显示在页面上，但会被浏览器解析。

meta元素通常用于指定网页的描述、关键词、文件的最后修改时间、作者和其他元数据

元数据可以使用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词）或其他Web服务

meta一般放置于head区域。

13.CSS可以通过以下方式添加到HTML中：

- 内联样式 - 在HTML元素中使用style属性
- 内部样式表 - 在HTML文档头部<head>区域使用<style>元素来包含CSS
- 外部引用 - 使用外部CSS文件

14.使用text-align(文字对齐)属性指定文本的水平与垂直对齐方式

15.指定图像的高度和宽度是一个很好的习惯。如果图像指定了高度宽度，页面加载时就会保留指定的尺寸。如果没有指定图片的大小，加载页面时有可能会破坏HTML页面的整体布局。

16.表格结构语义标签：

table ：表格根元素  thead表格头 tbody 表格体 tfoot表格尾 tr表格行

th表头单元行  td表体单元格 单元格内边距 cellpadding 单元格外边距 cellspacing 

col 和 colgroup用于便捷定义表格指定列的样式

17.

ol 定义有序列表

ul 定义无序列表

li 定义列表项

dl 定义列表

dt 自定义列表项目

dd 自定义列表项的描述

18.HTML 框架

通过使用框架，你可以在同一个浏览器窗口中显示不止一个页面。

```
<iframe src='URL'></iframe>
```

出于有些网页不希望被嵌套，响应头中有一选项， X-Frame-Options 它有三个可配置值

DENY:表示该网页不允许被嵌套，即便是在自己的域名的页面中也不能进行嵌套

SAMEORIGIN:表示该页面可以在相同域名页面中被嵌套展示

ALLOW-FROM uri: 表示该页面可以在指定来源页面中进行嵌套展示

19.HTML颜色

HTML颜色由红色、绿色、蓝色混合而成。

HTML颜色由一个十六进制符号来定义，这个符号由红色、绿色和蓝色的值组成(RGB)。

RGBA的意思是(Red-Green-Blue-Alpha)它是在RGB上扩展包括了“alpha”通道，运行对颜色值设置透明度。

20.HTML脚本标签

```
<script> 定义了客户端脚本
<noscript>定义了不支持脚本浏览器输出的文本
```

21.XHTML

XHTML是以XML格式编写的HTML。

什么 XHTML?

- XHTML指的是可扩展超文本标记语言
- XHTML与HTML4.01几乎是相同的
- XHTML是更严格更纯净的HTML版本
- XHTML是以XML应用的方式定义的HTML
- XHTML是2001年1月发布的W3C推荐标准
- XHTML得到所有主流浏览器的支持

XML是一种必须正确标记且格式良好的标记语言。

通过结合XML和HTML的长处，开发出了XHTML。XHTML是作为XML被重新设计的HTML。

与HTML相比最重要的区别：

### 文档结构

- XHTML DOCTYPE 是*强制性的*
- <html> 中的 XML namespace 属性是*强制性的*
- <html>、<head>、<title> 以及 <body> 也是*强制性的*

### 元素语法

- XHTML 元素必须*正确嵌套*
- XHTML 元素必须始终*关闭*
- XHTML 元素必须*小写*
- XHTML 文档必须有*一个根元素*

### 属性语法

- XHTML 属性必须使用*小写*
- XHTML 属性值必须用*引号包围*
- XHTML 属性最小化也是*禁止的*

```
<!DOCTYPE ...>是强制性的
XHTML元素必须合理嵌套
XHTML元素必须有关闭标签
空元素必须包含关闭标签
XHTML元素必须是小写
属性名称必须是小写
属性值必须有引号
不允许属性简写
```

22.HTML5设计的目的是为了在移动设备上支持多媒体。

HTML5是下一代HTML标准。

```
<!doctype>声明必须位于HTM5文档中的第一行。
<!DOCTYPE html>
```

HTML5定义了8个新的HTML语义元素。所有这些元素都是块级元素。

header、section、footer、aside、nav、main、article、figure

HTML5 <canvas>元素用于图形的绘制，通过脚本（通常是JavaScript）来完成。

23.HTML5内联SVG

## 什么是SVG？

- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用于定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
- SVG 是万维网联盟的标准

------

## SVG优势

与其他图像格式相比（比如 JPEG 和 GIF），使用 SVG 的优势在于：

- SVG 图像可通过文本编辑器来创建和修改
- SVG 图像可被搜索、索引、脚本化或压缩
- SVG 是可伸缩的
- SVG 图像可在任何的分辨率下被高质量地打印
- SVG 可在图像质量不下降的情况下被放大

## SVG 与 Canvas两者间的区别

SVG 是一种使用 XML 描述 2D 图形的语言。

Canvas 通过 JavaScript 来绘制 2D 图形。

SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。

在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

Canvas 是逐像素进行渲染的。在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

------

## Canvas 与 SVG 的比较

下表列出了 canvas 与 SVG 之间的一些不同之处。

| Canvas                                                       | SVG                                                          |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 依赖分辨率                                                                       不支持事件处理器                                                        弱的文本渲染能力                                                                                能够以 .png 或 .jpg 格式保存结果图像                               最适合图像密集型的游戏，其中的许多对象会被频繁重绘 | 不依赖分辨率                                                          支持事件处理器                                                    最适合带有大型渲染区域的应用程序（比如谷歌地图）                                                                    复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）                                                           不适合游戏应用 |

24.HTML5 MathML

HTML5可以在文档中使用MathML元素，对应的标签是<math>...</math>

MathML是数学标记语言，是一种基于XML（标准通用标记语言的子集）的标准，用来在互联网上书写数学符号和公式的置标语言。

25.HTML5拖放（Drag 和 Drop）

拖放（Drag和Drop）是HTML5标准的组成部分。

拖放是一种常见的特性，即抓取对象以后拖到另一个位置。

在HTML5中，拖放是标准的一部分，任何元素都能够拖放。

26.web worker是运行在后台的JavaScript，不会影响页面的性能。

27.服务器发送事件（Server-Sent Events）,允许网页自动获取来自服务器的更新。

28.HTML5 WebSocket 

WebSocket是HTML5开始提供的一种在单个TCP连接上进行全双工通讯的协议。

WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务器端主动向客户端推送数据。在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就可以创建持久性的连接，并进行双向数据传输。

29.在HTML5中，哪个元素用于组合标题元素？ <hgroup>

html5中，不在支持 <font> <acronym> rel

在HTML5中，contextmenu和spellcheck是：HTML属性

哪种输入类型定义滑块控件 ？ range

在SVG中，允许三种图像对象存在，分别是矢量图像、点阵图像和文本。

具有type属性的input元素，其值为“tel”，表示用于输入电话号码的单行纯文本编辑控件

<keygen>标签规定用于表单的密钥对生成器字段。当提交表单时，私钥存储在本地，公钥发送到服务器。

突出显示元素指定文本的重要性，mark元素表示标记或高亮显示以供参考的文档中的文本。

<track>标签为媒体元素(<audio> and <video>)规定外部文本轨道。这个元素用于规定字幕文件或其他包含文本的文件，当媒体播放时，这些文件是可见的。

<param>元素允许为插入XHTML文档的对象规定run-time设置，也就是说，此标签可为包含它的<object>标签提供参数。

外部资源由 <object> 元素表示，该元素可以被视为图像、嵌套的浏览上下文或插件要处理的资源。它包括各种属性，如存档、边框、分类、代码库、代码类型等

HTML5删除了 <big> <center> <font> <tt> <strike>

如果没有设置高度和宽度，浏览器就不知道视频的大小。其效果是，当视频加载时，页面将发生变化（或闪烁）

在 HTML 音频/视频 DOM 中，_____设置或返回音频/视频播放的默认速度？ defaultPlaybackRate

html的link标签是用于当前文档引用外部文档用的，rel属性用于设置对象和链接目的间的关系

target的默认属性是_self (默认在当前窗口打开)

浏览器中激活链接的默认颜色是：红色

<area>标签定义图像映射中的区域（注：图像映射指的是带有可点击区域的图像）

在XHTML文档中哪些元素是强制性的？ doctype、html、head、body以及title

HTML5是XHTML的升级版本。（错误）

插件的功能是扩展HTML浏览器的功能。

插件可以通过<object>标签或者<embed>标签添加在页面中。

所有主流浏览器都支持<object>标签。<object>元素定义了在HTML文档中嵌入的对象。

```
<embed>定义内嵌对象。HTML4中不赞成，HTML5中允许
<object>定义内嵌对象
<param>定义对象的参数
<audio>定义了声音内容
<video>定义一个视频或者影片
<source>定义了media元素的多媒体资源
<track>规定media元素的字幕文件或其他包含文本的文件
```

30.HTML语言代码

HTML的lang属性可用于声明网页或部分网页的语言。这对搜索引擎和浏览器是有帮助的。

根据W3C推荐标准，应该通过<html>标签中的lang属性对每张页面中的主要语言进行声明，比如：

```
<html lang='en'>...</html>
```

在XHTML中，采用如下方式在<html>标签中对语言进行声明：

```
<html xmlns='http://www.w3.org/1999/xhtml' lang='en' xml:lang='en'>...</html>
```

31.HTTP 方法

GET 从指定的资源请求数据

POST 向指定的资源提交要被处理的数据

HEAD 与get相同，但只返回HTTP报头，不返回文档主体

PUT 上传指定的URI表示

DELETE 删除指定的资源

OPTIONS 返回服务器支持的HTTP方法

CONNECT 把请求连接转换到透明的TCP/IP通道
