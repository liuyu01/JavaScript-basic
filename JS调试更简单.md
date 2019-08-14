##### 1.console

console.log()

console.dir()

console.warn() 输出字的颜色是黄色的

console.table()以一种比仅仅转出原始对象数组更整洁的方式显示表格数据。

console.assert() 对输入的表达式进行断言，只有表达式为false时，才输出相应的信息到控制台。

console.count() 计数器，可以统计代码被执行的次数。

console.trace()试图在类或库中找出是哪个实际调用者导致这个问题

console.time()一个用于跟踪操作时间的专用函数，它是跟踪JavaScript执行时间的好方法。

console.group()

##### 2.选择DOM元素

$('tagName') $('.class') $('#id') and $('.class #id') 等效于`document.querySelector('')`，这将返回 DOM 中与选择器匹配的第一个元素。

可以使用 $$(tagName) 或 $$(.class), **注意双元符号**，根据特定的选择器选择DOM的所有元素。这也将它们放入数组中，你也可以通过指定数组中该元素的位置来从中选择特定的元素。

##### 3.将浏览器转换为编辑器

document.body.contentEditable = true;

##### 4.查找与DOM中的元素关联的事件

getEventListeners($('selector'))返回一个对象数组，其中包含绑定到该元素的所有事件。

要找到特定事件的侦听器，可以这样做：

getEventListeners($('selector')).eventName[0].listener

##### 5.监控事件

如果希望在执行绑定到DOM中特定元素的事件时监视它们，也可以在控制台中这样做。你可以使用不同的命令来监控其中的一些或所有事件:

- `monitorEvents($(‘selector’))` 将监视与选择器的元素关联的所有事件，然后在它们被触发时将它们打印到控制台。例如，`monitore($(#firstName))` 将打印 **ID** 为 `firstName`元素的所有事件。
- `monitorEvents($(‘selector’),’eventName’)` 将打印与元素绑定的特定事件。 你可以将事件名称作为参数传递给函数。 这将仅记录绑定到特定元素的特定事件。 例如，monitorEvents($(‘#firstName’),’click’) 将打印绑定到ID为'firstName'的元素的所有 `click` 事件。
- `monitore($(selector)，[eventName1, eventName3'， .])`将根据您自己的需求记录多个事件。与其传递单个事件名作为参数，不如传递包含所有事件的字符串数组。例如`monitore($(#firstName)，[click, focus])`将记录与ID firstName元素绑定的 `click`事件和`focus`事件。
- `unmonitorevent ($(selector))`:这将停止监视和打印控制台中的事件。

##### 6.检查DOM中的一个元素

你可以直接从控制台检查一个元素:

- inspect((‘#firstName’))`将检查 ID为'firstName' 的元素，`spect($(‘a’)[3](https://link.juejin.im?target=https%3A%2F%2Fwww.fundebug.com%2F))`将检查 DOM 中的第 4 个`a` 元素。
- $0, $1, $2 等可以帮助你获取最近检查过的元素。 例如，`$0` 表示最后检查的 DOM 元素，而`$1` 倒数第二个检查的 DOM 元素。

##### 7.检索最后一个结果的值

你可以将控制台用作计算器。当你这样做的时候，你可能需要用第二个来跟踪一个计算。以下是如何从内存中检索先前计算的结果:

```
$_
```

##### 8.清除控制台和内存

如果你想清除控制台及其内存，输入如下：

clear()
