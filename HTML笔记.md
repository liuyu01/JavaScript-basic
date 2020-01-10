###### HTML中href、src区别：

href是Hypertext Reference的缩写，表示超文本引用。用来建立当前元素和文档之间的链接。

src是source的缩写，src的内容是页面必不可少的一部分，是引入。src指向的内容会嵌入到文档中当前标签所在的位置。当浏览器解析到该元素时，会暂停浏览器的渲染，直到该资源加载完毕。

简而言之，src用于替换当前元素；href用于在当前文档和引用资源之间建立联系。

###### HTML框架

通过使用框架，可以在同一个浏览器窗口中显示不止一个页面。

```
<iframe src='URL'></iframe> 该URL指向不同的网页。
```

height 和 width 属性用来定义iframe标签的高度与宽度。属性默认以像素为单位，但是你可以指定其按比例显示（如： 80%）

frameborder属性用于定义iframe表示是否显示边框。

iframe可以显示一个目标链接的页面

###### HTML颜色

###### 由一个十六进制符号来定义，这个符号由红色、绿色和蓝色的值组成（RGB）。

相对于使用rgb(255,255,0)，使用rgba(255,255,0,0.5)可以实现设置颜色透明度的功能，这里的0.5表示透明度，范围0~1，0表示全透明。

<noscript>标签提供无法使用脚本时的替代内容，比如在浏览器禁用脚本时，或浏览器不支持客户端脚本时。

<noscript>元素可包含普通HTML页面的body元素中能够找到的所有元素。

在HTML中不能使用小于号（<）和大于号(>)，这是因为浏览器会误认为它们是标签。

```
不间断空格（Non-breaking Space） HTML中的常用字符实体是不间断空格（&nbsp;)
```

###### HTML 统一资源定位器 （Uniform Resource Locators）

URL是一个网页地址。可以由字母组成或互联网协议(IP)地址

URL只能使用ASCII字符集来通过因特网进行发送。由于URL常常会包含ASCII集合之外的字符，URL必须转换为有效的ASCII格式。URL编码使用%其后跟随两位的十六进制数来替换非ASCII字符。URL不能包含空格。URL编码通常使用+来替换空格。

###### XHTML是以XML格式编写的HTML.

什么是XHTML?

- XHTML指的是可扩展超文本标记语言
- XHTML与HTML 4.01几乎是相同的
- XHTML是更严格更纯净的HTML版本
- XHTML是以XML应用的方式定义的HTML
- XHTML是2001年1月发布的W3C推荐标准
- XHTML得到所有主流浏览器的支持

与HTML相比最重要的区别：

文档结构

- XHTML DOCTYPE是强制性的
- <html>中的XML namespace属性是强制性的
- <html>、<head>、<title>以及<body>也是强制性的

元素语法

- XHTML元素必须正确嵌套
- XHTML元素必须始终关闭
- XHTML元素必须小写
- XHTML文档必须有一个根元素

属性语法

- XHTML属性必须使用小写
- XHTML属性值必须用引号包围
- XHTML属性最小化也是禁止的

如何将HTML转换为XHTML

1. 添加一个XHTML<!DOCTYPE>到你的网页中
2. 添加xmlns属性添加到每个页面的html元素中
3. 改变所有的元素为小写
4. 关闭所有的空䛾
5. 修改所有的属性名称为小写
6. 所有属性值添加引号

HTML5是HTML最新的修订版本，2014年10月由万维网联盟（W3C）完成标准制定。

HTML5的设计目的是为了在移动设备上支持多媒体。

HTML5中的一些有趣的新特性：

- 用于绘画的canvas元素
- 用于媒介回放的video和audio元素
- 对本地离线存储的更好的支持
- 新的特殊内容元素，比如article、footer、header、nav、section
- 新的表单控件，比如calendar、date、time、email、url、search

```
<!doctype html>声明必须位于HTML5文档中的第一行
```

HTML5的改进： 新元素、新属性、完全支持CSS3、Video和Audio、2d/3d制图、本地存储、

本地SQL数据、web应用

HTML5 <canvas>元素用于图形的绘制，通过脚本（通常是JavaScript）来完成。

<canvas>标签只是图形容器，您必须使用脚本来绘制图形。</canvas>

getContext("2d")对象是内建的HTML5对象，拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。

###### HTML5支持内联SVG。

什么是SVG？

- SVG指可伸缩矢量图形（Scalable Vector Graphics）
- SVG用于定义用于网络的基于矢量的图形
- SVG使用XML格式定义图形
- SVG图像在放大或改变尺寸的情况下其图形质量不会有损失
- SVG是万维网联盟的标准

SVG优势（与其他图形格式相比比如jpeg 和 gif）,使用svg的优势在于：

- SVG图像可通过文本编辑器来创建和修改
- SVG图像可被搜索、索引、脚本化或压缩
- SVG是可伸缩的
- SVG图像可在任何的分辨率下被高质量地打印
- SVG可在图像质量不下降的情况下被放大

SVG与Canvas两者的区别：

SVG 是一种使用 XML 描述 2D 图形的语言。

Canvas 通过 JavaScript 来绘制 2D 图形。

SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。

在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

Canvas 是逐像素进行渲染的。在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

下表列出了 canvas 与 SVG 之间的一些不同之处。

| Canvas                                                       | SVG                                                          |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1.依赖分辨率                                                           2.不支持事件处理器                                                                    3.弱的文本渲染能力                                                                                 4.能够以 .png 或 .jpg 格式保存结果图像                                                   5.最适合图像密集型的游戏，其中的许多对象会被频繁重绘 | 1.不依赖分辨率                                                     2.支持事件处理器                                                     3.最适合带有大型渲染区域的应用程序（比如谷歌地图）                                                                    4.复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）                                                         5.不适合游戏应用 |

###### HTML5 MathML

HTML5可以在文档中使用MathML元素，对应的标签是<math>...</math>.

MathML是数学标记语言，是一种基于XML（标准通用标记语言的子集）的标准，用来在互联网上书写数学符号和公式的置标语言。

###### HTML5拖放（Drag和Drop）

拖放（Drag和Drop）是HTML5标准的组成部分。

拖放是一种常见的特性，即抓取对象以后拖到另一个位置。

在HTML5中，拖放是标准的一部分，任何元素都能够拖放。

###### HTML5 geolocation(地理定位)

HTML5 Geolocation API用于获得用户的地理位置。鉴于该特性可能侵犯用户的隐私，除非用户同意，否则用户位置信息是不可用的。

使用getCurrentPosition()方法来获得用户的位置。

navigator.geolocation.getCurrentPosition(showPosition);

###### HTML5 Video(视频)

video元素提供了播放、暂停和音量空间来控制视频。同时，<video>元素也提供了width和height属性控制视频的尺寸。如果设置了高度和宽度，所需的视频空间会在页面加载时保留。如果没有设置这些属性，浏览器不知道大小的视频，浏览器就不能再加载时保留特定的空间，页面就会根据原始视频的大小而改变。

video标签之间插入的内容是提供给不支持video元素的浏览器显示的。

video元素支持多个<source>元素。<source>元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式。

###### HTML5 Audio(音频)

control属性供添加播放、暂停和音量控件。

在<audio> 与 </audio> 之间你需要插入浏览器不支持的<audio>元素的提示文本 。

audio元素允许使用多个 <source> 元素. <source> 元素可以链接不同的音频文件，浏览器将使用第一个支持的音频文件。

###### HTML5 新的Input类型

Input类型：color  color 类型用在input字段主要用于选取颜色

Input类型：date  date类型允许你从一个日期选择器选择一个日期

Input类型：datetime  datetime类型允许你选择一个日期（UTC时间）

Input类型：datetime-local  datetime-local类型允许你选择一个日期和时间（无时区）

Input类型：email  email类型用于应该包含e-mail地址的输入域

Input类型：month month类型允许你选择一个月份

Input类型：number  number类型用于应该包含数值的输入域

Input类型：range range类型用于应该包含一定范围内数字值的输入域。range类型显示为滑动条

Input类型：search search类型用于搜索域

Input类型：tel  tel类型用于电话号码

Input类型：time  time类型允许你选择一个时间

Input类型：url   url类型用于应该包含URL地址的输入域

Input类型：week week类型允许你选择周和年

###### HTML5新的表单元素

datalist <input>标签定义选项列表。清淤<input>元素配合使用该元素，来定义input可能的值

keygen  <keygen>标签规定用于表单的密钥对生成器字段

output  <output>标签定义不同类型的输出，比如脚本的输出

###### HTML5表单属性

autocomplete属性规定form或input域应该拥有自动完成功能。当用户在自动完成域中开始输入时，浏览器应该在该域中显示填写的选项。

autocomplete 适用于 <form> 标签，以及以下类型的 <input> 标签：text, search, url, telephone, email, password, datepickers, range 以及 color。

form novalidate属性  
novalidate属性是一个boolean(布尔)属性。novalidate属性规定在提交表单时不应该验证form或input域。

<input> autofocus 属性

autofocus属性是一个boolean属性。autofocus属性规定在页面加载时，域自动地获得焦点。

<input> form属性

form属性规定输入域所属的一个或多个表单。

<input> formaction属性

formaction属性会覆盖<form>元素中的action属性

<input>formenctype属性

formenctype属性描述了表单提交到服务器的数据编码（只对form表单中method='post'表单）

formenctype属性覆盖form元素的enctype属性

<input> formmethod属性

formmethod 属性定义了表单提交的方式

formmethod属性覆盖了<form>元素的method属性

<input> formnovallidate属性

novalidate 属性是一个 boolean 属性.

novalidate属性描述了 <input> 元素在表单提交时无需被验证。

formnovalidate 属性会覆盖 <form> 元素的novalidate属性.

<input> formtarget属性

formtarget 属性指定一个名称或一个关键字来指明表单提交数据接收后的展示

formtarget属性覆盖<form>元素的target属性

<input>height和width属性

height 和 width 属性规定用于 image 类型的 <input> 标签的图像高度和宽度。

**注意:** height 和 width 属性只适用于 image 类型的<input> 标签。

**提示:**图像通常会同时指定高度和宽度属性。如果图像设置高度和宽度，图像所需的空间 在加载页时会被保留。如果没有这些属性， 浏览器不知道图像的大小，并不能预留 适当的空间。图片在加载过程中会使页面布局效果改变 （尽管图片已加载）。

<input> list属性

list属性规定输入域的datalist。datalist是输入域的选项列表。

<input> min和max属性

min、max 和 step 属性用于为包含数字或日期的 input 类型规定限定（约束）。

**注意:** min、max 和 step 属性适用于以下类型的 <input> 标签：date pickers、number 以及 range。

<input> multiple属性

multiple 属性是一个 boolean 属性.

multiple 属性规定<input> 元素中可选择多个值。

**注意:** multiple 属性适用于以下类型的 <input> 标签：email 和 file:

<input> pattern属性

pattern 属性描述了一个正则表达式用于验证 <input> 元素的值。

**注意:**pattern 属性适用于以下类型的 <input> 标签: text, search, url, tel, email, 和 password.

<input>placeholder属性

placeholder 属性提供一种提示（hint），描述输入域所期待的值。

简短的提示在用户输入值前会显示在输入域上。

**注意:** placeholder 属性适用于以下类型的 <input> 标签：text, search, url, telephone, email 以及 password。

<input>required 属性

required 属性是一个 boolean 属性.

required 属性规定必须在提交之前填写输入域（不能为空）。

**注意:**required 属性适用于以下类型的 <input> 标签：text, search, url, telephone, email, password, date pickers, number, checkbox, radio 以及 file。

<input> step属性

step 属性为输入域规定合法的数字间隔。

如果 step="3"，则合法的数是 -3,0,3,6 等

**提示：** step 属性可以与 max 和 min 属性创建一个区域值.

**注意:** step 属性与以下type类型一起使用: number, range, date, datetime, datetime-local, month, time 和 week.

form 定义一个form表单

input 定义一个input域

###### HTML5语义元素

语义元素 = 有意义的元素  一个语义元素能够清楚的描述其意义给浏览器和开发者

HTML5 <section>元素

section 标签定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。

HTML5 <article>元素

article标签定义独立的内容

HTML5 <nav>元素

nav标签定义导航链接的部分。nav元素用于定义页面的导航链接部分区域，但是，不是所有的链接都需要包含在<nav>元素中。

HTML5 <aside>元素

aside标签定义页面主区域内容之外的内容（比如侧边栏）。aside标签的内容应与主区域的内容相关。

HTML5 <header>元素

header元素描述了文档的头部区域。header元素主要用于定义内容的介绍展示区域。在页面中可以使用多个header元素。

HTML5 <footer>元素

footer元素描述了文档的底部区域。一个页脚通常包含文档的作者，著作权信息，链接的使用条款，联系信息等。文档中可以使用多个footer元素。

HTML5 <figure>和<figcaption>元素

figure标签规定独立的流内容（图像、图表、照片、代码等等）。

figure元素的内容应该与主内容相关，但如果被删除，则不应对文档流产生影响。

figcaption标签定义figure元素的标题。

figcaption元素应该被置于figure元素的第一个或最后一个元素的位置。

###### HTML5 Web 存储

HTML5 web存储，一个比cookie更好的本地存储方式。

使用HTML5可以在本地存储用户的浏览数据。

早些时候，本地存储使用的是cookie。但是Web存储需要更加的安全与快速，这些数据不会被保存在服务器上，但是这些数据只用于用户请求网站数据上。它也可以存储大量的数据，而不影响网站的性能。数据以键/值对存在，web网页的数据只允许该网页访问使用。

localStorage和sessionStorage

- localStorage -- 用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除
- sessionStorage  -- 用于临时保存同一窗口（或标签页）的数据，在关闭窗口或标签页之后将会删除这些数据。

不管是 localStorage，还是 sessionStorage，可使用的API都相同，常用的有如下几个（以localStorage为例）：

- 保存数据：localStorage.setItem(key,value);
- 读取数据：localStorage.getItem(key);
- 删除单个数据：localStorage.removeItem(key);
- 删除所有数据：localStorage.clear();
- 得到某个索引的key：localStorage.key(index);

`localStorage`只要在相同的协议、相同的主机名、相同的端口下，就能读取/修改到同一份localStorage数据。

`sessionStorage`比`localStorage`更严苛一点，除了协议、主机名、端口外，还要求在同一**窗口**（也就是浏览器的标签页）下。

###### HTML5应用程序缓存

使用HTML5，通过创建cache manifest文件，可以轻松地创建web应用的离线版本。

什么是应用程序缓存（Application Cache）？

HTML5引入了应用程序缓存，这意味着web应用可进行缓存，并可在没有因特网连接时进行访问。

应用程序缓存为应用带来三个优势：

1. 离线浏览 - 用户可在应用离线时使用它们
2. 速度 - 已缓存资源加载得更快
3. 减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源

HTML5 Web Workers

web worker 是运行在后台的JavaScript,不会影响页面的性能。

什么是Web Worker ？

当在HTML页面中执行脚本时，页面的状态是不可响应的，知道脚本已完成。

web worker 是运行在后台的JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时web worker在后台运行。

###### HTML5 服务器发送事件（Server-Sent Events）

HTML5服务器发送事件（server-sent event)允许网页获得来自服务器的更新。

Server-Sent事件 - 单向消息传递

Server-Sent事件指的是网页自动获取来自服务器的更新。

以前也可能做到这一点，前提是网页不得不询问是否有可用的更新。通过服务器发送事件，更新能够自动到达。

EventSource对象用于接收服务器发送事件通知。

###### HTML5 WebSocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。

现在，很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

HTML5 定义的 WebSocket 协议，能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。

说到 websocket 我觉得有必要说下跟 socket 的区别。

软件通信有七层结构，下三层结构偏向与数据通信，上三层更偏向于数据处理，中间的传输层则是连接上三层与下三层之间的桥梁，每一层都做不同的工作，上层协议依赖与下层协议。基于这个通信结构的概念。

Socket 其实并不是一个协议，是应用层与 TCP/IP 协议族通信的中间软件抽象层，它是一组接口。当两台主机通信时，让 Socket 去组织数据，以符合指定的协议。TCP 连接则更依靠于底层的 IP 协议，IP 协议的连接则依赖于链路层等更低层次。

WebSocket 则是一个典型的应用层协议。

总的来说：Socket 是传输控制层协议，WebSocket 是应用层协议。

