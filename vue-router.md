###### vue-router路由信息meta

在全局守卫beforeEach((to, from, next) => {...})中判断当前路由是否允许登录、是否需要身份认证、权限认证等，虽然可以采用路由匹配的方式 if(to.path === '/url')，很显然当需要验证的路由较多时，会增加太多的if判断，这不利于代码维护，此时可在定义路由的时候可以配置meta字段，通过设置一些属性值，可以便于我们对当前的路由做一些处理，也可以使用next()重定向到其他路由。

使用，在定义路由时定义一个需要验证的meta信息 meta: {requiresAuth: true}

路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录，例如，根据下面的路由配置，/foo/bar这个URL将会匹配父路由记录以及子路由记录。

```
const router = new VueRouter({
	routes: [
		{
		    path: '/foo',
		    component: Foo,
		    children: [
		    	{
		    		path: 'bar',
		    		component: Bar,
		    		meta: {requiresAuth: true}
		    	}
		    ]
		}
	]
})
```

一个路由匹配到的所有路由记录会暴露为$route对象的$route.matched数组。因此，我们需要遍历$route.matched来检查路由记录中的meta字段。

然后再全局导航守卫中检查元字段是否需要验证，

```
router.beforeEach((to, from, next)=>{
	if(to.matched.some(record=>record.meta.requiresAuth)){
		// this route requires auth, check if logged in
    	// if not, redirect to login page.
		if(!auth.loggedIn()){
			next({
				path: '/login',
				query: {redirect: to.fullPath}
			})
		}else{
			next()
		}
	}else{
		next() //确保一定要调用next()
	}
})
```



###### 路由懒加载

为什么要使用路由懒加载呢?

用vue.js写单页面应用时，会出现打包后的JavaScript包非常大，影响页面加载，我们可以利用路由的懒加载去优化这个问题，当我们用到某个路由后，才去加载对应的组件，这样就会更加高效。

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

结合 Vue 的[异步组件](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)和 Webpack 的[代码分割功能](https://doc.webpack-china.org/guides/code-splitting-async/#require-ensure-/)，轻松实现路由组件的懒加载。

首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：

```
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })
```

第二，在 Webpack 2 中，我们可以使用[动态 import](https://github.com/tc39/proposal-dynamic-import)语法来定义代码分块点 (split point)

```
import('./Foo.vue') // 返回 Promise
```

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```
const Foo = () => import('./Foo.vue')
```

在路由配置中什么都不需要改变，只需要像往常一样使用 `Foo`

```
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

用法：

```
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
export default new Router({
  routes: [
    {
      path: '/',
	  meta: {
       requiresAuth: true
      },
      component: resolve => require(['components/Hello.vue'], resolve)
    },
    {
        path: '/about',
        component: resolve => require(['components/About.vue'], resolve)
    }
  ]
})
```

###### 把组件按组划分

有时候我们想把某个路由下的所有组件都打包在同个异步块（chunk）中。只需要使用命名chunk，一个特殊的注释语法来提供chunk name(需要 Webpack > 2.4).

```
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack会将任何一个异步模块与相同的块名称组合到相同的异步块中。

###### redirect 重定向

“重定向”的意思是，当用户访问/a时，URL将会被替换成/b，然后匹配路由为/b。

```
{
path: '/gohome', //路由路径
redirect: '/'    //重定向的路径就是要跳转的那个页面的路径
}
```

redirect的路径就是我们要跳转到的组件的路径

重定向也是通过routes配置来完成，下面例子是从/a 重定向到 /b；

```
const router = new VueRouter({
	routes:[
		{ path: '/a', redirect: '/b'}
	]
})
```

重定向的目标也可以是一个命名的路由：

```
const router = new VueRouter({
	routes: [
		{ path: '/a', redirect: { name: 'foo'}}
	]
})
```

甚至是一个方法，动态返回重定向目标：

```
const router = new VueRouter({
	routes: [
		{
			path: '/a', redirect: to => {
				//方法接收 目标路由 作为参数
				// return 重定向的 字符串路径/路径对象
			}
		}
	]
})
```

注意导航守卫并没有应用在跳转路由上，而仅仅应用在其目标上。在下面这个例子中，为/a路由添加一个 beforeEnter 守卫并不会有任何效果。

###### 别名

/a的别名是/b，意味着，当用户访问/b时，URL会保持为/b，但是路由匹配则为/a，就想用户访问/a一样。

上面对应的路由配置为：

```
const router = new VueRouter({
	routes: [
		{ path: '/a', component: A, alias: '/b'}
	]
})
```

“别名”的功能让你可以自由地将UI结构映射到任意的URL，而不是受限于配置的嵌套路由结构。

###### path与name的区别？

```
this.$router.replace({
	name: 'securityRequirements',
	query: this.getQuery(),
})
与
this.$router.replace({
    path: 'sources',
    query: {
         query: this.query || undefined,
 
    },
})
有什么区别？

name 指的是对应路由下的路由名称，
path 指的是对应路由的路径
```

可参考路由router/index.js下的配置

![image-20201020112330432](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201020112330432.png)

用js实现跳转时有两种对应组合：

```javascript
	this.$router.push({'name':one; params:{'id':id}});
	this.$router.push({'path':'/home'; query:{'id':id}});
```

也就是name和params组合使用，path和query组合使用。

区别：其实name和query也可以组合实现页面跳转，但是参数无法正常传递接收，
params传递参数在地址栏是看不到的，就跟post请求很像。

query参数会显示在地址栏，就跟get请求很像。

query类似 get, 跳转之后页面 url后面会拼接参数,类似?id=1, 非重要性的可以这样传, 密码之类还是用params，刷新页面id还在。

params类似 post, 跳转之后页面 url后面不会拼接参数 , 但是刷新页面id 会消失

###### vue路由跳转的带参与不带参,路由跳转传参方式：name 、 path

方法一：用**path**跳转加参数，相当于get请求，页面可以看到你传参

方法二：用name跳转，params传参，相当于post请求，页面不可以看到你传参值

###### 路由传递参数和传统传递参数是一样的，命名路由类似表单提交而查询就是url传递：

1.命名路由搭配params，刷新页面参数会丢失

2.查询参数搭配query，刷新页面数据不会丢失

3.接收参数使用this.$router后面就是搭配路由的名称就能获取到参数的值

######  path:'name'和path:'/name'的区别

```
//比如当前路径是home
this.$router.push({path:'name'})//==>path为/home/name
this.$router.push({path:'/name'})//==>path为/name
```

###### path 和 Name路由跳转方式，都可以用query传参

###### replace 与push 的区别

push方法会向history栈添加一个新的记录，而replace方法是替换当前的页面，不会向history栈添加一个新的记录

this.$router.push跳转到指定url路径，并想history栈中添加一个记录，点击后退会返回到上一个页面。

this.$router.replace 跳转到指定url路径，但是history栈中不会有记录，点击返回会跳转到上上个页面 (就是直接替换了当前页面)。

this.$router.go(n) 向前或者向后跳转n个页面，n可为正整数或负整数。

配置路由时path有时会加'/'有时不加，以‘/’开头的会被当作根路径，就不会一直嵌套之前的路径。

###### this.$router和this.$route的区别

`this.$router`是VueRouter的实例方法，当导航到不同url，可以使用`this.$router.push`方法，这个方法则会向history里面添加一条记录，当点击浏览器回退按钮或者`this.$router.back()`就会回退之前的url。
`this.$route`相当于当前激活的路由对象，包含当前url解析得到的数据，可以从对象里获取一些数据，如name,path等。

###### 路由跳转分为编程式和声明式

声明式

```
//路由入口
<router-link :to="...">
//视图出口
<router-view></router-view>
```

编程式

```
// 字符串
router.push('home')
// 对象
router.push({ path: 'home' })
// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})
// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

注意：如果提供了path, params会被忽略，上述例子中的query并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的name或手写完整的带有参数的path:

```
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

同样的规则也适用于router-link组件的to属性。

###### $route对象

![2019070516324236.png](https://img.jbzj.com/file_images/article/201907/2019070516324236.png)

$route对象表示当前的路由信息，包含了当前 URL 解析得到的信息。包含当前的路径，参数，query对象等。
**1.$route.path**
   字符串，对应当前路由的路径，总是解析为绝对路径，如 "/foo/bar"。
**2.$route.params**
   一个 key/value 对象，包含了 动态片段 和 全匹配片段，
   如果没有路由参数，就是一个空对象。
**3.$route.query**
   一个 key/value 对象，表示 URL 查询参数。
   例如，对于路径 /foo?user=1，则有 $route.query.user == 1，
   如果没有查询参数，则是个空对象。
**4.$route.hash**
   当前路由的 hash 值 (不带 #) ，如果没有 hash 值，则为空字符串。锚点
**5.$route.fullPath**
   完成解析后的 URL，包含查询参数和 hash 的完整路径。
**6.$route.matched**
   数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。
**7.$route.name  当前路径名字**
**8.$route.meta 路由元信息**

###### $router对象

![2019070516324337.png](https://img.jbzj.com/file_images/article/201907/2019070516324337.png)

$router对象是全局路由的实例，是router构造方法的实例。

路由实例方法： 

push 、 go 、replace 、

###### vue路由守卫beforeEach、钩子

总体来讲vue里面提供了三大类钩子

1.全局钩子函数

2.单个路由钩子钩子函数（路由独享的守卫）

3.组件内钩子函数

###### vue-router全局钩子函数

beforeEach和afterEach是vue-router实例对象的属性。特别提醒：每次路由跳转，都会执行beforeEach和afterEach

router.beforeEach有三个参数：to/from/next

- **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
- **`from: Route`**: 当前导航正要离开的路由
- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。

```
router.beforeEach(function (to,from,next) {
// to: Route: 即将要进入的目标 路由对象
// from: Route: 当前导航正要离开的路由
// next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
  next();
})
```

router.afterEach有两个参数：to/from，这些钩子不会接受 `next` 函数也不会改变导航本身

```
router.afterEach(function (to,from) {
  console.log(to);//到达的路由
  console.log(from);//离开的路由
})
```

这类钩子主要作用于全局,一般用来判断权限,以及以及页面丢失时候需要执行的操作。

###### 单个路由钩子函数

给某个路由单独使用的，本质上和后面的组件内钩子是一样的。都是特指的某个路由。不同的是，这里的一般定义在router当中，而不是在组件内。

```
{
path: '/dashboard',
component: resolve => require(['../components/page/Dashboard.vue'], resolve),
meta: { title: '系统首页' },
beforeEnter: (to, from, next) => {

},
},
```

###### 组件路由钩子函数

主要包括 beforeRouteEnter和beforeRouteUpdate ,beforeRouteLeave,这几个钩子都是写在组件里面也可以传三个参数(to,from,next),作用与前面类似.

```
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

记住**参数或查询的改变并不会触发进入/离开的导航守卫**。

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

###### [#](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#完整的导航解析流程)完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

###### 过渡动效

<router-view>是基本的动态组件，所以我们可以用<transition>组件给它添加一些过渡效果。

```
<transition>
	<router-view></router-view>
</transition>
```

单个路由的过渡

上面的用法会给所有路由设置一样的过渡效果，如果你想让每个路由组件有各自的过渡效果，可以在各路由组件内使用 `<transition>` 并设置不同的 name。

```
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}
```

基于路由的动态过渡

还可以基于当前路由与目标路由的变化关系，动态设置过渡效果

```
<!-- 使用动态的 transition name -->
<transition :name="transitionName">
  <router-view></router-view>
</transition>
```

```
// 接着在父组件内
// watch $route 决定使用哪种过渡
watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
}
```

###### 数据获取

有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

- 导航完成之后获取： 先完成导航，然后再接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
- 导航完成之前获取：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

从技术角度讲，两种方式都不错---就看你响应的用户体验是哪种。

###### 滚动行为

使用前端路由，当切换到新路由时，想要页面滚到底部，或者是保持原先的滚到位置，就像重新加载页面那样。vue-router能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

注意：这个功能只在支持 history.pushState的浏览器中可用。

当创建一个Router实例，你可以提供一个scrollBehavior方法：

```
const router = new VueRouter({
	routes: [...],
	scrollBehavior(to, from, savedPosition){
		//return 期望滚动到哪个的位置
	}
})
```

`scrollBehavior` 方法接收 `to` 和 `from` 路由对象。第三个参数 `savedPosition` 当且仅当 `popstate` 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

这个方法返回滚动位置的对象信息，长这样：

- `{ x: number, y: number }`
- `{ selector: string, offset? : { x: number, y: number }}` (offset 只在 2.6.0+ 支持)

如果返回一个 falsy (译者注：falsy 不是 `false`，[参考这里](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy))的值，或者是一个空对象，那么不会发生滚动。

留意一下 `this.$router` 和 `router` 使用起来完全一样。我们使用 `this.$router` 的原因是我们并不想在每个独立需要封装路由的组件中都导入路由。

动态路由匹配：使用:标记。

###### 响应路由参数的变化

提醒一下，当使用路由参数时，例如从/user/foo导航到/user/bar，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。

复用组件时，想对路由参数的变化作出响应的话，你可以简单地watch（监测变化）$route对象。

###### 捕获所有路由或404 Not Found 路由

常规参数只会匹配被/分隔的URL片断中的字符。如果想匹配任意路径，可以使用通配符(*)。

当使用通配符路由时，请确保路由的顺序是正确的，也就是说含有通配符的路由应该放在最后。

当使用一个*通配符*时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过*通配符*被匹配的部分

###### 匹配优先级

有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。

注意：在Vue实例内部，你可以通过$router访问路由实例。因此你可以调用this.$router.push。

###### 命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar` (侧导航) 和 `main` (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 `router-view` 没有设置名字，那么默认为 `default`。

```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 s)：

```
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

###### HTML5 History 模式

vue-router默认hash模式---使用URL的hash来模拟一个完整的URL，于是当URL改变时，页面不会重新加载。

如果不想要很丑的hash，我们可以用路由的history模式，这种模式充分利用history.pushStateAPI来完成URL跳转而无须重新加载页面。

```
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，也好看！

不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404，这就不好看了。

所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 app 依赖的页面。



<router-link>

<router-view>  显示的是当前路由地址所对应的内容

在vue-router中, 我们看到它定义了两个标签<router-link> 和<router-view>来对应点击和显示部分。<router-link> 就是定义页面中点击的部分，<router-view> 定义显示部分，就是点击后，区配的内容显示在什么地方。所以 <router-link> 还有一个非常重要的属性 to，定义点击之后，要到哪里去， 如：<router-link to="/home">Home</router-link>

执行过程：当用户点击 router-link 标签时，会去寻找它的 to 属性， 它的 to 属性和 js中配置的路径{ path: '/home', component: Home} path 一一对应，从而找到了匹配的组件， 最后把组件渲染到
<router-view> 标签所在的地方。所有的这些实现才是基于hash 实现的。

###### 路由嵌套总结

任何子路由都是在其父路由的组件中切换显示，不管是多少层的路由嵌套，都是这样的理解，所以父路由需要有以下两点，二者缺一不可

- 有组件引用
- 组价中有router-view组件

https://zhuanlan.zhihu.com/p/95074683
