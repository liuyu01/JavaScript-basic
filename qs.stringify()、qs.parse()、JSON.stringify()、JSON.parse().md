qs.stringify()、qs.parse()、JSON.stringify()、JSON.parse()

###### qs.stringify()、qs.parse()的使用

qs是npm仓库所管理的包，可以通过npm install qs安装。

qs.stringify()作用是将对象或数组序列化成URL的格式。以&进行拼接。

```
//对象序列化
let obj = {
	methods: 'query_stu',
	id: 1,
	name: 'chenchen'
};
qs.stringify(obj);
//   methods=query_stu&id=1&name=chenchen    这就是我们的传到服务器的url
```

```
//数组序列化
let arr = [2, 3];
qs.stringify({a:arr})
//'arr[0]=2&arr[1]=3'
```

这种格式可以进行转为序列化，但是url中会带有数组的下标a[0]、a[1]，这并不是我们一般的出来了办法。常用方法如下：

```
//常用并推荐使用
let arr = [2, 3];
qs.stringify({a:arr},{indices:false});
//'arr=2&arr=3'注意这个格式，一般是我们常用的格式哦
```

其中indices:false，去除默认处理的方式。如果不写这个的话。则默认是第一种处理的方式（带下标）

当然，也可以通过`arrayFormat` 选项进行格式化输出，如下代码所示：

```
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'indices' })
// 'a[0]=b&a[1]=c'
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'brackets' })
// 'a[]=b&a[]=c'
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'repeat' })
// 'a=b&a=c'
```

qs.parse()则就是反过来，将我们通过qs.stringify()序列化的对象或数组转回去。

```
let url = 'id=1&name=chenchen'
qs.stringify(url)
// {id:1，name:chenchen}  
```

###### JSON.stringify

JSON的常规用途是同web服务器进行数据交换。

在向web服务器发送数据时，数据必须是字符串。

JSON.stringify() 方法用于将 JavaScript 值转换为 JSON 字符串。

JSON.parse()可以将JSON字符串转为一个对象。

```
var a = {name:'hehe',age:10};
 qs.stringify(a)
// 'name=hehe&age=10'
JSON.stringify(a)
// '{"name":"hehe","age":10}'
JSON.parse('{"name":"hehe","age":10}')
//{name:'hehe',age:10}
```

###### lodash pickBy

```
_.pickBy(object, [predicate=_.identity])
```

创建一个从object中经predicate判断真值的属性为对象。predicate会传入1个参数：(value)

参数

1. object (Object)

   来源对象

2. [predicate=_.identity] (Function|Object|string)

   这个函数会调用每一个属性

返回值 *(Object)*

返回新对象

```
var object = { 'a': 1, 'b': '2', 'c': 3 };

_.pickBy(object, _.isNumber);
// => { 'a': 1, 'c': 3 }
```

###### lodash debounce

```
_.debounce(func, [wait=0], [options])
```

创建一个防抖动函数。该函数会在wait毫秒后调用func方法。该函数提供一个cancel方法取消延迟的函数调用以及flush方法立即调用。可以提供一个options对象决定如何调用func方法，options.leading与|或options.trailing决定延迟前后如何触发。func会传入最后一次传入的参数给防抖动函数。随后调用的防抖动函数返回是最后一次func调用的结果。

**注意:** 如果 `leading` 和 `trailing` 都设定为 true。 则 func 允许 trailing 方式调用的条件为: 在 wait 期间多次调用防抖方法。

###### 参数

1. func (Function)

   要防抖动的函数

2. [wait=0] (number)

   需要延迟的毫秒数

3. [options] (Object)

   选项对象

4. [options.leading=false] (boolean)

   指定调用在延迟开始前

5. [options.maxWait] (number)

   设置 `func` 允许被延迟的最大值

6. [options.trailing=true] (boolean)

   指定调用在延迟结束后

返回值 (Function)

返回具有防抖动功能的函数

```
// 避免窗口在变动时出现昂贵的计算开销。
jQuery(window).on('resize', _.debounce(calculateLayout, 150));

// 当点击时 `sendMail` 随后就被调用。
jQuery(element).on('click', _.debounce(sendMail, 300, {
  'leading': true,
  'trailing': false
}));

// 确保 `batchLog` 调用1次之后，1秒内会被触发。
var debounced = _.debounce(batchLog, 250, { 'maxWait': 1000 });
var source = new EventSource('/stream');
jQuery(source).on('message', debounced);

// 取消一个 trailing 的防抖动调用
jQuery(window).on('popstate', debounced.cancel);
```

