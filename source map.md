source map

source map文件是js文件压缩后，文件的变量名替换对应、变量所在位置等元信息数据文件，一般这种文件和min.js主文件放在同一个目录下。 比如压缩后原变量是map，压缩后通过变量替换规则可能会被替换成a，这时source map文件会记录下这个mapping的信息，这样的好处就是说，在调试的时候，如果有一些JS报错，那么浏览器会通过解析这个map文件来重新merge压缩后的js,使开发者可以用未压缩前的代码来调试，这样会给我们带来很大的方便！ 而这种还原性调试功能，目前只有chorme才具有，所以就会出现标题说的问题，我引入jquery-1.10.2.min.js的时候，在firefox下或者其他浏览器下是好的，在chorme下会报错找不到jquery-1.10.2.min.map文件，404，就是因为以上说的情况，jquery会检测浏览器是否支持source map功能，如果支持的话，那就去下载source map文件，而这个时候如果你引用的是官网的min.js那没问题，它会去自己的目录下找source map文件，而如果jquery.min.js文件在你的服务器上而服务器上又没有source map的话，那就会报错了，所以三种解决办法：一，引用官网文件 二，把source map文件下载下来放到服务器上(推荐) 三，把chorme文件的工具–>开发者工具–>设置–>Enable source maps勾去掉，去掉这个勾，jquery就不会去下载source map文件了！



在使用bootstrap时发现，浏览器访问的的路径中包含这么一个请求 bootstrap.css.map。因为bootstrap的css是用less编写的，所以在浏览器里调试的时候为了确保css代码和less中代码对应关系，就有了这个map文件。这样就方便针对性的进行修改了。



bootstrap.css 和 boostrap.min.css 的区别就是：前者是未压缩的，后者是压缩过的。

一般情况下，未压缩的用于开发环境，如果程序出错方便调试。而压缩过的用生生产环境，大大减少网络数据传输量，便之更快更节约流量……

boostrap.min.css也是完整的库，只不过通过工具压缩，令相同的代码所占空间更小，从而提升web性能，减少宽带的压力，然而这是以文件的可读性为代价的。当你打开min文件之后会发现代码挤在一起几乎没办法阅读。压缩的方法就是删除了所有换行和空格。

Bootstrap3和Bootstrap4最大的区别在于Bootstrap4现在使用flexbox（弹性盒子）而不是浮动。Flexbox的一大优势是，没有指定宽度的网格将自动设置为等宽与等高。

Bootstrap4默认的font-size为16px，line-height为1.5。

CSS3弹性盒子（Flex Box）

弹性盒子是CSS3的一种新的布局模式。CSS3弹性盒（Flexible Box或flexbox），是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。引入弹性盒布局模型的目的是提供一种更加有效的方式来对一个容器的字元素进行排列、对齐和分配空白空间。

弹性盒子由弹性容器（Flex container）和弹性子元素（Flex item）组成。

弹性容器通过设置display属性的值为flex或inline-flex将其定义为弹性容器。

弹性容器内包含了一个或多个弹性子元素。

弹性容器外及弹性子元素内是正常渲染的。弹性盒子只定义了弹性子元素如何在弹性容器内布局。

弹性子元素通常在弹性盒子内一行显示。默认情况每个容器只有一行。

