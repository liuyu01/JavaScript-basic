##### vue-router路由传参

用params传参，F5强制刷新参数会被清空，用query,由于参数适用路径传参的，所以F5强制刷新也不会被清空。也可以选用sessionStorage/localStorage/cookie存储

###### 1.params(URL不显示参数)、F5刷新后参数消失

（1）路由配置(index.js)

```
routes: [
	{
		path: '/',
		component: A
	},{
		path: '/b',
		name: 'b', //这句特别重要，params必须用name来识别路径
		component: B
	}
]
```

（2）按钮事件

```
<button @click='goToB'>路由跳转</button>
```

采用编程式跳转。把要传递的参数放在params里，data可以是具体值，也可以是本组件的变量。**特别要注意**，必须给要跳转的组件在配置路由时就要添加一个name，尝试在index.js里不带name,在b组件接收参数时就找不到。

```
goToB() {
	this.$router.push({
		path: "/b",
		name: 'b',
		params: {
			data: 1,
		}
	})
}
```

在b组件里接收参数。使用this.$route.params,其中data

```
created(){
	console.log(this.$route.params.data)
}
```

**注意：**刷新b组件后参数会消失。

###### sessionStorage:容量大、安全、临时存储、跨页面

###### localStorage:容量大、安全、永久存储、跨页面

###### 2.params(URL不显示参数)、F5刷新后参数不消失

```
<!-- 存储页面 test-local -->
<template>
  <div>
    <a @click="toAnother()">点击</a>
  </div>
</template>
<script>
export default {
  methods: {
    toAnother() {
      // session 添加session
      sessionStorage.code = 90088
      sessionStorage.setItem('page', 1)
      // local 添加local
      localStorage.age = 27
      localStorage.setItem('passcode', 12345)
      this.$router.push(`/testLocalTo`)
    }
  }
}
</script>
```

```
<!-- 跳转页面 test-local-to -->
<template>
  <div></div>
</template>
<template>
  <div></div>
</template>
<script>
export default{
  created() {
    this._getLocalData()
  },
  methods: {
    _getLocalData() {
      // session 获取session
      let code = sessionStorage.code
      console.log('code =', code)
      let page = sessionStorage.getItem('page')
      console.log('page =', page)
      // local 获取local
      let age = localStorage.age
      console.log('age =', age)
      let passcode = localStorage.getItem('passcode')
      console.log('passcode =', passcode)
    }
  }
}
</script>
```

页面刷新后，参数依然在

关闭当前页面，再次打开后，sessionStorage消失，localStorage依旧有。

###### cookie:容量小、不安全、有时间期限、跨页面

###### 3.params（URL显示参数）

(1)路由配置

```
export default new Router({
	routes: [
	{
		path: '/',
		component: A
	},{
		path: '/b/:data', //本行是为了路由的模式匹配
		name: 'b',
		component: B
	}]
})
```

(2)按钮事件

```
<button @click='goToB(data)'>路由跳转</button>
```

(3)编程跳转

```
methods: {
	goToB(data) {
		this.$router.push({
			path: `/b/${data}`, //本行对应路由格式配置，data可以是实值，可以是本地变量
		})
	}
}
```

获取参数的方式：

```
created() {
	console.log(this.$route.params.data)
}
```

**注意：**此种传参方式会把参数显示在URL中，通常用于路径跳转的需求。

###### 4.使用query

(1)路由

```
routes: [
    {
      path: '/',
      component: A
    },
    {
      path: '/b',
      component: B
    }
  ]
```

(2)按钮

```
<button @click="goToB(data)">路由跳转</button>
```

(3)编程跳转

可以看到只是params变成了query，其他的和第一种很相似。只不过这种会在地址栏显示参数而已

```
 goToB(data) {
      this.$router.push({
        path: `/b`,
        query:{
          data
        }
      });
    },
```

获取参数的方法：console.log(this.$route.query.data)

结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200917113125535.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjg2NTI5,size_16,color_FFFFFF,t_70#pic_center)

由于query与params传参机制不一样，造成的差异，如果要隐藏参数用params，如果强制刷新不被清除用query

////

先有如下场景，点击当前页的某个按钮跳转到另外一个页面去，并将某个值带过去

```
<el-button type='primary' @click="handleClick(2)">查看详情</el-button>
```

###### 1.第一种方法：拼接方式

```
methods: {
	handleClick(id){
		//直接调用$router.push 实现带参数的跳转
		this.$router.push({
			path: `/detail/${id}`
		})
	}
}
```

对应路由配置：

```
{
	path: '/detail/:id',
	name: 'detail',
	component: detail
},

```

获取参数方式：

```
this.$route.params.id
```

这种方式传参，页面刷新数据不会丢失。

###### 2.第二种方法：params传参

通过路由属性中的name来确定匹配的路由，通过params来传递参数。

```
methods: {
	handleClick(id){
		this.$router.push({
			name: 'detail',//根据name确定匹配路由
			params: {
				id: id
			}
		})
	}
}
//或者采用router-link
<router-link :to="{name: 'detail', params: {id: 1}}">前往detail页面<router-link>
```

对应路由配置：

```
{
	path: '/detail/:id',
	name: 'detail',
	component: detail
}
```

获取参数方式：

```
this.$route.params.id
```

**需要注意的是，params动态路由传参，一定要在路由中定义参数，然后再路由跳转的时候必须要加上参数，否则就是空白页面。例如：**

```
//定义的路由中，只定义一个id参数
{
	path: 'detail/:id',
	name: 'detail',
	component: detail
}

//template中的路由传参，传了一个id参数和一个token参数，id是在路由中已经定义的参数，而token没有定义
<router-link :to="{name: 'detail', params: {id: 1, token: '123456'}}">前往detail页面</router-link>

//在详情页接收
created(){
	//以下都可以正常获取到
	//但是页面刷新后，id依然可以获取，而token此时就不存在了
	const id = this.$route.params.id;
	const token = this.$route.params.token;
}
```

###### 3.第三种方法：query传参

使用path来匹配路由，然后通过query来传递参数，这种情况下query传递的参数会显示在url后面?id=?

```
methods: {
	handleClick(id){
		this.$router.push({
			path: '/detail',
			query: {
				id: id
			}
		})
	}
}
```

对应路由配置：

```
{
	path: '/detail',
	name: 'detail',
	component: detail
}
```

获取参数：

```
this.$route.query.id
```

###### 4.总结：params和query中的区别

1.接收方式

query传参：this.$route.query.id

params传参：this.$route.params.id

2.路由展现方式

query传参：/detail?id=1&user=123&identity=1&更多参数

params传参： /detail/123
