javascript中的 document.body.clientHeight和 document.documentElement.clientHeight 的区别，我们都知道
document.body.clientWidth ==> BODY对象宽度
document.body.clientHeight==> BODY对象高度
document.documentElement.clientWidth ==> 可见区域宽度
document.documentElement.clientHeight==> 可见区域高度

经过测试发现在ie浏览器中会出现一些差别，主要是IE9及IE9以下，在这些低版本的IE浏览器中document.body.clientHeight]要比document.documentElement.clientHeight值要小。

另外如果当页面上没有DOCTYPE的时候,document.documentElement.clientHeight的值将变为0

原因是: 在HTML 中, body 是整个DOM 的根, 而在XHTML 中, document 才是根, body 不再是根, 所以取body 的属性时, 不能再取到整个页面的值.

XHTML中用 document.documentElement.clientHeight 代替 document.body.clientHeight

```undefined
height：指元素内容的高度，jQuery中的height()方法返回的就是这个高度。
clientHeight：内容高度+padding高度，jQuery中的innerHeight()方法返回的就是这个高度。
offsetHeight：内容高度+padding高度+边框宽度，jQuery中的outerHeight()方法返回的就是这个高度。

width：指元素内容的宽度，jQuery中的width()方法返回的就是这个宽度。
clientWidth：内容高度+padding宽度，jQuery中的innerWidtht()方法返回的就是这个宽度。
offsetWidth：内容高度+padding高度+边框宽度，jQuery中的outerWidth()方法返回的就是这个宽度。
```