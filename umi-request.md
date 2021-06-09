##### umi-request

umi-request 是基于fetch封装的开源http请求库，旨在为开发者提供一个统一的API调用方式，同时简化使用方式，提供了请求层常用的功能：

- URL参数自动序列化
- POST数据提交方式简化
- Response返回处理简化
- 请求超时处理
- 请求缓存支持
- GBK编码处理
- 统一的错误处理方式
- 请求取消支持
- Node环境http请求
- 拦截器机制
- 洋葱中间件机制

与fetch、axios的异同？

![preview](https://pic4.zhimg.com/v2-96ed949aa1475afeaf383668f2d41037_r.jpg)

umi-request 底层抛弃了设计粗糙、不符合关注分离的XMLHttpRequest，选择了更加语义化、基于标准Promise实现的fetch；同时同构更方便，使用isomorphic-fetch(目前已内置)；而基于各业务应用场景提取常见的请求能力并支持快速配置如post简化、前后缀、错误检查等。

实例化通用配置

请求一般都有一些通用的配置，我们不想在每个请求里去逐个添加，例如通用的前缀、后缀、头部信息、异常处理等等。那么可以通过extend来新建一个umi-request实例，从而减少重复的代码量。

https://github.com/umijs/umi-request/blob/master/README_zh-CN.md 文档地址

网络请求库，基于fetch封装，兼具fetch和axios的所有特点，具有缓存，超时，字符编码处理，错误处理等常用功能。

