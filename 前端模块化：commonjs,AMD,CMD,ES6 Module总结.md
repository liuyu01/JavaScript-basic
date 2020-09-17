##### 前端模块化：**commonjs**,AMD,CMD,ES6 Module总结

模块化的理解：模块化是一种处理复杂系统分解为更好的可管理模块的方式。简单来说就是解耦，简化开发，一个模块就是实现特定功能的文件，可以更方便地使用别人的代码，想要什么功能，就加载什么模块。模块开发需要遵循一定的规范

###### CommonJS规范

commonjs
暴露：
module.exports=value;
exports.xxx=value;
引入：
require('./')或者require('underscore')  为什么有的带./有的不带？ **同步加载**
./在自定义的模块中找，直接是名字的在node_modules中查找

特点：1. 模块输出的是一个值的拷贝， 模块是运行时加载，同步加载

　　　2.CommonJS 模块的顶层`this`指向当前模块

**加载方式**：同步加载，根据模块在代码中出现的先后顺序加载；也就是说，只有加载完成，才能执行后面的操作。这样就会对浏览器造成阻塞。而服务器端的文件相当于在本地存储，不存在请求网络等问题。故commonJs规范被认为不适合浏览器环境而适合服务器端；

**执行特点**：第一次加载时执行，并缓存执行结果，输出的是一个值的拷贝，即一旦输出一个值，模块内部的变化就影响不到这个值，自然也没有动态更新；

###### AMD规范

AMD 即 Asynchronous Module Definition，中文名是“异步模块定义”的意思。它是一个在浏览器端模块化开发的规范，AMD 是 RequireJS 在推广过程中对模块定义的规范化产出，所以AMD规范的实现，就是的require.js了

**特点**：依赖前置，所有依赖都在模块定义时声明。不管是在define中声明的依赖，还是在callback中通过require加载的依赖，都会先加载完再去执行callback；故它属于运行时加载。这个特性就导致了在模块定义时被声明的依赖，虽然没有被用到，也做了加载执行的操作；

特点 ：异步加载，不阻塞页面的加载，能并行加载多个模块，但是不能按需加载，必须提前加载所需依赖

AMD规范 专门用于客户端模块化的规范，模块的**加载是异步形式**

Require.js - 实现AMD的规范的一个第三方库

Amd 的规范中定义了两个重要的api

```
define(id?,[]?,callbakc): //定义声明模块，参数id 模块id标识(可选)，参数二是一个数组（可选），依赖其他模块，最后是回调函数

require([module],callback):// 加载模块，参数一，是数组，指定加载的模块，参数二回调函数，模块加载完成后执行
```

还有一个配置属性API:

```
require.config({

baseUrl: //基本路径

paths：// 对象，对外加载的模块名称  ： 键值关系，键：自定义模块名称，值 ：文件名或者文件路径(不要写文件后缀.js),可以是字符串，数组（如果第一个加载失败，会加载第二个）

shim：//对象，配置非AMD 模式的文件，每个模块要定义（1）exports：值（指在js文件暴露出来的全局变量，如：window.a）（2）deps： 数组，表明该模块的依赖性

})
```

###### CMD规范

CMD规范是阿里的玉伯提出来的，实现js库为sea.js。 它和requirejs非常类似，即一个js文件就是一个模块，但是CMD的加载方式更加优秀，是通过按需加载的方式，而不是必须在模块开始就加载所有的依赖。

Common Module Definition 通用模块定义，CMD规范的概念是随着sea.js的推出产生的。同时，sea.js也借鉴了很多require.js的东西。它与 CommonJS 和 Node.js 的 Modules 规范保持了很大的兼容性。通过 CMD 规范书写的模块，可以很容易在 Node.js 中运行；

**特点**：同AMD一样，CMD也属于运行时加载。但是CMD规范中，依赖模块可以通过require.async在需要的地方引入并执行（懒加载）。这个特性使它可以实现按需加载。

```
seajs.config({
//设置别名，方便调用
alias: { 'jquery': ' http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js' }
});
define(function(require, exports, module) {
    //引用jQuery模块
    var $ = require('jquery');
});
// 加载多个模块，在加载完成时，执行回调
seajs.use(['./a', './b'], function(a, b) {
  a.doSomething();
  b.doSomething();
});
```

###### ES6的module规范

**特点**：与AMD和CMD不同的是，它的设计理念是尽量的静态化，在编译阶段就能确定模块的依赖关系，输入输出等，即编译时加载(静态加载);与commonJs提供的是值的拷贝不同得是，export向外提供的是一个只读的动态引用，故通过import加载的模块，是不会被缓存的。这一特点也说明ES6是支持动态更新的。

ES6在语言标准的层面上，实现了模块功能，而且非常简单，ES6到来，完全可以取代CommonJS和AMD规范，成为浏览器和服务器通用的模块解决方案。

特点：1.ES6模块之中，顶层的this指向undefined，即不应该在顶层代码使用this

2.自动采用严格模式“use strict”。须遵循严格模式的要求

3.ES6模块的设计思想是尽量的静态化，编译时加载或者静态加载，编译时输出接口

4.ES6模块export、import命令可以出现在模块的任何位置，但是必须处于模块顶层。如果处于块级作用域内，就会报错

5.ES6模块输出的是值得引用

模块功能主要由两个命令构成：export 和 import。

**比较一下默认输出和正常输出**

```
//第一组
export default function crc32(){//输出
	//...
}
import crc32 from 'crc32'; //输入

//第二组
export function crc32(){//输出
	//...
}
import {crc32} from 'crc32'; //输入
```

分析：上面代码的两组写法，

第一组是使用export default时，对应的import语句不需要使用大括号；

第二组是不使用export default时，对应的import语句需要使用大括号。

浏览器加载规则：

1.浏览器加载ES6模块，也使用<script>标签，但是要加入type='module'属性。

浏览器对于带有type='module'的<script>，都是异步加载，不会造成堵塞浏览器，

```
<script type='module' src='./foo.js'></script>
```

2.ES6模块也允许内嵌在网页中，语法行为与加载外部脚本完全一致。

```
<script type='module'>
	import utils from './utils.js'
	//other code
</script>
```



##### 模块化使用对比

### CommonJS 和 ES6 Module

1. CommonJS 模块输出的是值的拷贝，ES6 模块输出的是值的引用
   - CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。
   - ES6 模块输出的是值的引用，模块里面的内容如果发生变化，输出的内容也会随之改变。
2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口
   - 运行时加载：CommonJS 模块就是对象；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。
   - 编译时加载：ES6 模块不是对象，而是通过 export 命令显式指定输出的代码，import时采用静态命令的形式。即在import时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。

### AMD 和 CMD

AMD：在最开始声明依赖模块，加载模块后马上运行模块化内代码，不管回调函数是否要用到相关模块
 CMD：在需要时声明依赖模块、加载模块
