##### Vue provide()的用法

```
  components: {
    container
  },
  provide () {
    return {
      contain: this
    };
  },
  data () {
    return { }
  }
```

provide和inject主要为高阶插件/组件库提供用例。并不推荐直接用于应用程序代码中。

是2.2.0版本新增的。

这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。

provide选项应该是一个对象或返回一个对象的函数。该对象包含可注入其子孙的属性。在该对象中你可以使用ES2015 Symbols作为key，但是只在原生支持Symbol和Reflect.ownKeys的环境下可工作。

inject选项应该是：一个字符串数组，或一个对象，对象的key是本地的绑定名，value是：在可用的注入内容中搜索用的key(字符串或Symbol)，或一个对象，该对象的：form属性是在可用的注入内容中搜索用的key(字符串或Symbol)  default属性是降级情况下使用的value

###### 使用场景：由于vue有$parent属性可以让子组件访问父组件。但孙组件想要访问祖先组件就比较困难。通过provide/inject可以轻松实现跨级访问祖先组件的数据。

一种最常见的用法是刷新vue组件

app.vue

```
<template>
  <div
    id="app"
  >
    <router-view
      v-if="isRouterAlive"
    />
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
    MergeTipDialog,
    BreakNetTip
  },
  data () {
    return {
      isShow: false,
      isRouterAlive: true
  },

// 父组件中返回要传给下级的数据
  provide () {
    return {
      reload: this.reload
    }
  },
  methods: {
    reload () {
      this.isRouterAlive = false
      this.$nextTick(() => {
        this.isRouterAlive = true
      })
    }
  }
}
</script>
```

```
<template>
  <popup-assign
    :id="id"
    @success="successHandle"
  >
    <div class="confirm-d-tit"><span class="gray-small-btn">{{ name }}</span></div>
    <strong>将被分配给</strong>
    <a
      slot="reference"
      class="unite-btn"
    >
      指派
    </a>
  </popup-assign>
</template>
<script>
import PopupAssign from '../PopupAssign'
export default {
//引用vue reload方法
  inject: ['reload'],
  components: {
    PopupAssign
  },
methods: {
    // ...mapActions(['freshList']),
    async successHandle () {
      this.reload()
    }
  }
}
</script>
```

这样就实现了子组件调取reload方法就实现了刷新vue组件的功能，个人认为它实现了组件跨越组件传递数据方法。

###### 祖先组件不需要知道哪些后代组件使用它提供的属性

###### 后代组件不需要知道被注入的属性来自哪里

##### 提示：provide和inject绑定并不是可响应的。这是刻意为之的。然而，如果你传入了一个可监听的对象，那么其对象的属性还是可响应的。

provide链：之所以加一个链字因为就是它的行为跟原型链是一致的-----沿着链向上寻找，只要找到就停止。