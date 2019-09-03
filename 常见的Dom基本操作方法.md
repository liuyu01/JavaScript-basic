##### 常见的Dom基本操作方法

##### 文档元素选取

###### 1.ID选择器 

document.getElementById()

###### 2.名称选择器

document.getElementsByName()

###### 3.标签选择器

document.getElementsByTagName()

###### 4.类选择器

document.getElementsByClassName()

###### 5.CSS单元素选择器

document.querySelector()

###### 6.CSS多元素选择器

document.querySelectorAll()

##### DOM遍历

###### 节点相关

1.父节点 parentNode

2.子节点 childNodes 返回所有子节点，即NodeList对象

3.首子节点  firstChild  返回第一个子节点

4.最后子节点 lastChild 返回最后一个子节点

5.下一兄弟节点 nextSibling  返回下一个兄弟节点

6.前一兄弟节点  previousSibling 返回前一个兄弟节点

7.节点类型  nodeType 返回节点类型的数字表示

1--代表Element节点

3--代表Text节点

8--代表Comment节点

9--代表Document节点

11--代表DocumentFragment节点

8.节点值  nodeValue  返回Text节点或Comment节点的值

9.节点名  nodeName  返回元素的标签名，以大写形式表示

###### 元素相关

1.子元素 children  返回所有子元素

2.首子元素 firstElementChild  返回所有子元素中的第一个（节点是元素的一种）

3.尾子元素 lastElementChild   返回所有子元素中的最后一个

4.下一兄弟元素 nextElementSibling  返回下一个兄弟元素

5.上一兄弟元素 previousElementSibling 返回前一个兄弟元素

5.子元素数量  childElementCount

##### 属性

###### 标准属性

表示HTML文档元素的HTMLElement对象定义了读/写属性，它们对应于元素的HTML属性。HTMLElement定义的通用HTML属性，包括id、lang、dir、事件处理程序onclick及表单相关属性等。

###### 非标准属性

1.获取属性值  getAttribute

2.属性值设置  setAttribute

3.属性存在检测  hasAttribute

4.删除属性  removeAttribute

5.数据集属性  dataset

在HTML5文档中，任意以data-为前缀的小写的属性名字都是合法的。这些“数据集属性”定义了一种标准的、附加额外数据的方法。

6.元素属性  attributes

##### 元素内容

1.元素内容 innerHTML

2.元素及内容 outerHTML

3.纯文本元素内容  textContext

##### 创建节点

1.创建元素节点 createElement

2.创建文本节点 createTextNode

3.创建文档片段 createDocumentFragment

4.创建注释节点  createComment

5.节点克隆 cloneNode

##### 插入节点

1.插入子节点  appendChild

2.节点前插入 insertBefore

##### 删除和替换

删除子节点  removeChild

替换子节点  replaceChild
