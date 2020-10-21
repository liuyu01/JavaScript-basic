axios 笔记

axios的文档...发现它传参有两种方式，一种是params，一种是data，

而params会默认把 要传的参数添加到url后面

1. params会将请求的参数拼接到url中去，用于get请求
2. data是添加到请求体(body)里面去，用于post请求



post请求可以使用params方式传值

![image.png](https://upload-images.jianshu.io/upload_images/13899355-7ef3fbbf846c10fd.png&originHeight=239&originWidth=425&size=10502&status=done&style=none&width=425?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`/create?name=***&age=****`,但是是一个`post`请求

解决方法：

axios签名是`axios.post(url[, data[, config]])`。所以你想在第三个参数中发送params对象

```
axios.post(`/create`, null, { params:{name,age}})
.then(response => response.status)
.catch(err => console.warn(err) );
```



axios中delete 多样传参

//请求参数放在请求体中

```
axios.delete(`/delete`, {data: {id: 12}})
.then(res => {console.log(res) })
```

//请求参数拼接在url上用params

```
axios.delete(`/delete`, {params: {id: 12}})
.then(res => {console.log(res) })
```



1、export default 向外暴露的成员，可以使用任意变量来接收

2、在一个模块中，export default 只允许向外暴露一次

3、在一个模块中，可以同时使用export default 和export 向外暴露成员

4、使用export向外暴露的成员，只能使用{ }的形式来接收，这种形式，叫做【按需导出】

5、export可以向外暴露多个成员，同时，如果某些成员，在import导入时，不需要，可以不在{ }中定义

6、使用export导出的成员，必须严格按照导出时候的名称，来使用{ }按需接收

7、使用export导出的成员，如果想换个变量名称接收，可以使用as来起别名

8、在一个文件或模块中，`export`、`import`可以有多个，`export default`仅有一个

9、通过 `export` 方式导出，在导入时要加`{ }`，`export default` 则不需要



![image-20201019094518954](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201019094518954.png)

一般vue-router报错说明是路由配置出问题了，或者跳转路由的时候出现死循环，RangeError: Maximum call stack size exceeded 是死循环出现的语句。





some()方法用于检测数组中的元素是否满足指定条件（函数提供）。

some()方法会依次执行数组的每个元素：

如果有一个元素满足条件，则表达式返回true，剩余的元素不会再执行检测。如果没有满足条件的元素，则返回false。

some()不会对空数组进行检测。some()不会改变原始数组。

##### url传参

url出现了有+，空格，/，?，%,  #,  &,  =等特殊符号的时候，可能在服务器端无法获得正确的参数值，如何是好？

解决办法

将这些字符转化成服务器可以识别的字符，对应关系如下：

+URL中+号表示空格  %2B

空格 URL中的空格可以用+号或者编码 %20

/ 分隔目录和子目录  %2F

? 分隔实际的URL和参数 %3F

% 指定特殊字符              %25

#表示书签                        %23

&URL中指定的参数间的分隔符 %26

=URL中指定参数的值 %3D



关于一些url中传递参数有空格问题：

url.replace(/ /g, '%20');

从上面的例子中可以看到可以用：replace(/ /g, '%20')来替换url中参数的空格。url中的空格可以用+或者%20代替。



可以用js中的encodeURI(String)或encodeURIComponent(String)方法。

encodeURI: 对整个的url进行编码时使用。

encodeURIComponent:对某个url中的参数进行编码。

这个问题在使用加密工具加密参数后再传参时是很常见的，因为加密后的字符串里面经常带有加号+。

