---
layout: post
title:  "Vue-Router学习笔记"
categories: VUE
date:   2018-10-22 11:48:05
author: Logan
tags:  vue-router
---

* content
{:toc}

# 一、基础

## 1. 介绍

Vue Router 是 Vue.js官方的**路由管理器**。是我们使用Vue构建的SPA（单页应用 Single Page Application）的**路径管理器**。本质上就是建立**页面Url与组件之间的映射关系**。

## 2. 基本使用

### 2.1 安装

```cmd
npm install vue-router -S
```

### 2.2 在入口文件中引入并配置

```js
// 项目入口文件main.js

// 1. 引入vue、vue-router
import Vue from 'vue'
import VueRouter from 'vue-router'

// 2. 引入项目组件
import App from './App.vue'
import Home from './components/Home.vue'
import Hello from './components/Hello.vue'

// 3. 调用vue-router
Vue.use(VueRouter)

// 4. 创建router实例，定义路由配置
const router = new VueRouter({
  routes: [
    {
      path: '/home',
      component: Home
    },
    {
      path: '/hello',
      component: Hello
    },
    {
      path: '/',
      // 根目录重定向至 /home
      redirect: '/home'
    }
  ]
})

// 5. 创建、挂载根实例并注入路由
new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

### 2.3 在`App.vue`中插入导航及视图渲染标签

```html
<!-- App.vue -->
<template>
    <div>
      <div class="nav">
        <router-link to="/home">Home</router-link>
        <router-link to="/hello">Hello</router-link>
      </div>
      <!-- 路由渲染出口 -->
      <router-view></router-view>
    </div>
</template>
```





# 二、动态路由匹配（相当于路由传参）

**使用场景**

某个模式下匹配到的所有路由，都映射到同一组件。例如不同用户的个人信息页面，或者不同文章页面等。

**要点概括**

1. 定义路由路径时使用动态参数（以冒号开头）：`path: '/ComponentA/:xxx'`
2. 在`ComponentA`中通过`this.$route.params.xxx`获取参数

```js
// Main.js 
// 定义路由配置时路径使用动态参数（以冒号开头即可）
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User
    }
  ]
})

// User.vue
// 路由匹配成功时，参数会被设置到this.$route.params中
export default {
  ...
  // 组件内的路由导航守卫
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`，通过传回调给 next并把组件实例作为回调方法的参数来访问组件实例
    next(vm => {
      vm.getUserInfo(vm.$route.params.id)
    })
  },
  // 组件内的路由导航守卫
  beforeRouteUpdate(to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    this.getUserInfo(this.$route.params.id)
  }
}

// User.vue
// 在模板插值语法中，通过 $route.params.id 获取路径参数
<template>
  <div>
    <p>用户ID：{{$route.params.id}}</p>
    <p>用户名：{{userInfo.name}}</p>
  </div>
</template>
```

[查看在线例子](https://jsfiddle.net/logan70/nw2zbm6a/)

# 三、嵌套路由（多级路由）

**使用场景**

应用中组件存在多层嵌套组合的关系。

**要点概括**

1. 路由配置时使用`children`字段，配置类似`routes`
2. 在渲染组件中插入嵌套的导航`<router-link>`及渲染出口`<router-view>`标签

```js
// Main.js 
// 定义路由配置时路径使用children字段，配置类似routes
const router = new VueRouter({
  routes: [
    {
      path: '/user',
      component: User,
      children: [
        {path: 'info', component: UserInfo},
        {path: 'posts', component: UserPosts}
      ]
    }
  ]
})

// User.vue
// 在模板中，插入嵌套的<router-link>及<router-view>
<template>
  <div>
    <p>用户User组件</p>
    <p>
      <router-link to="/user/info">用户信息</router-link>
      <router-link to="/user/posts">用户文章</router-link>
    </p>
    <router-view></router-view>
  </div>
</template>
```

[查看在线例子](https://jsfiddle.net/logan70/3q9vawbz/)

# 四、命名视图（多个路由渲染出口）

**使用场景**

1. 同时 (同级) 展示多个视图
2. 嵌套视图的复杂布局。（将嵌套组件视为根组件，则等同于场景1，不过是在`routes`内配置或`children`内配置的区别）

**要点概括**

1. 配置多个路由渲染出口`<router-view>`并设置`name`属性
2. 路由配置时使用`components`字段代替`component`字段 **多个s**
3. `components`对象内，key为`<router-view>`标签的`name`属性（没设置时，默认为`default`），value为要渲染的组件

```js
// App.vue
// 在模板中，配置多个路由渲染出口`<router-view>`并设置`name`属性
<template>
  <div>
    <p>命名视图</p>
    <p>
      <router-link to="/">主页</router-link>
    </p>
    <router-view></router-view>
    <router-view name="view1"></router-view>
    <router-view name="view2"></router-view>
  </div>
</template>

// Main.js 
// 配置路由时使用components字段代替component字段，key为<router-view>标签的`name`属性（没设置时，默认为`default`），value为要渲染的组件
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Index,
        view1: View1,
        view2: View2
      }
    }
  ]
})
```

[查看在线例子](https://jsfiddle.net/logan70/jvydu2we/)

# 五、路由导航跳转方法总结

```js
mehtods: {
  routerMethodsCollection() {
    // router.push
    // 类似 window.history.pushState，会在history中产生新纪录，回退按钮可退回
    // 点击 <router-link> 内部调用router.push，故<router-link :to="..."> 对应 router.push(...)
    this.$router.push('/home') // 传入路径字符串
    this.$router.push({ path: 'home' }) // 传入路由描述对象
    // 路由传参，如果指定了path则会传参失败
    this.$router.push({ name: 'user', params: { userId: 123 }}) // 传入路由描述对象
    // 带查询参数，变成 /userlist?sex=male
    this.$router.push({ path: '/userlist', query: { sex: 'male' }})
    
    // router.replace
    // 类似 window.history.replaceState不会向 history 添加新记录，而是替换掉当前记录，其他与router.push无异
    // <router-link :to="..." replace> 对应	router.replace(...)
    this.$router.replace('/home') // 传入路径字符串
    this.$router.replace({ path: 'home' }) // 传入路由描述对象

    // router.go
    // 类似 window.history.go
    this.$router.go(1) // 前进一步，等同于 history.forward()
    this.$router.go(-1) // 后退一步，等同于 history.back()
    this.$router.go(-1) // 后退三步，等同于 history.go(-3)
  }
}
```

[查看在线例子](https://jsfiddle.net/logan70/1yftxzLn/)

# 六、路由传参

> 动态路由匹配也算是一种路由传参，详见第二大点`二、动态路由匹配（相当于路由传参）`

最常用的传参方式就是通过以下两种方式：

1. `<router-link>` 标签中的`to`属性的路由对象传入字段`params`
2. `router.push`中的路由对象传入字段`params`

上面两种方式本质上是相同的，只不过是**声明式和编程式**两种表现

*这里需要注意的是，传入的路由对象中，如果传入了`path`字段，则`params`字段**会被忽略**。所以当需要传参时，一般传入`name`字段来描述路由地址。*

```js
// Main.js 
// 配置路由时注意需传参的路由要进行命名
const router = new VueRouter({
  routes : [
    {path: '/home',component: Home},
    {path: '/param',component: Param, name: 'param'}
  ]
})

// App.vue
// 在模板中，配置路由渲染出口及传参
<template>
  <div>
    <p>路由传参</p>
    <p>
      <router-link to="/">主页</router-link>
      <router-link to="{name: 'param', params: {msg: '声明式路由跳转传入的参数'}}">声明式传参</router-link>
      <button @click="$router.push({name: 'param', params: {msg: '编程式路由跳转传入的参数'}})">编程式传参</button>
    </p>
    <router-view></router-view>
  </div>
</template>

// Param.vue 中接收参数
<template>
  <div>
    <p>接收的参数：{{$route.params.msg}}</p>
  </div>
</template>
```

[查看在线例子](https://jsfiddle.net/logan70/3yo1en2g/)

# 七、路由重定向和别名

```js
// 路由重定向
// 意味着访问/a会定向到/b，URL也会变为/b
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' },
    { path: '/c', redirect: {name: 'use'}}
  ]
})

// 路由别名
// 意味着访问/b会匹配到/a，URL保持为/b
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
    { path: '/a', component: A, alias: ['/b', '/c'] }
  ]
})
```

[查看在线例子](https://jsfiddle.net/logan70/ure83kpL/)

# 八、路由钩子函数（导航守卫）

- 全局前置守卫：beforeEach
- 全局解析守卫：beforeResolve
- 全局后置钩子：afterEach
- 路由守卫：beforeEnter
- 组件守卫：beforeRouteEnter
- 组件守卫：beforeRouteUpdate
- 组件守卫：beforeRouteLeave

```js
// Main.js中
const router = new VueRouter({
  routes: [
    {
      path: '/home',
      component: Home,
      // 【路由守卫beforeEnter】
      beforeEnter(to, from, next) {
        console.log('路由守卫：beforeEnter')
        // ...
        next() // 确保要调用 next 方法
      }
    }
  ]
})

// 【全局前置守卫beforeEach】
router.beforeEach((to, from, next) => {
  console.log('全局前置守卫：beforeEach')
  // ...
  next() // 确保要调用 next 方法，否则钩子就不会被 resolved
})

// 【全局解析守卫beforeResolve】
router.beforeResolve((to, from, next) => {
  console.log('全局解析守卫：beforeResolve')
  // ...
  next() // 确保要调用 next 方法，否则钩子就不会被 resolved
})

// 【全局后置钩子afterEach】
router.afterEach((to, from) => {
  console.log('全局后置钩子：afterEach')
  // ...
})

// Home.vue中
export default {
  // 【组件守卫beforeRouteEnter】
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`，可以传一个回调给 next，将组件实例作为参数来访问组件实例
    console.log('组件守卫：beforeRouteEnter')
    next(vm => {
      // vm即为组件实例
    })
  },

  // 【组件守卫beforeRouteUpdate】
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用，例如动态路由
    // 可以访问组件实例 `this`
    console.log('组件守卫：beforeRouteUpdate')
    next() // 确保要调用 next 方法
  },

  // 【组件守卫beforeRouteLeave】
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
    console.log('组件守卫：beforeRouteLeave')
    next() // 确保要调用 next 方法
  },
  created() {
    console.log('组件生命周期：created')
  },
  mounted() {
    console.log('组件生命周期：mounted')
  }
}
```

**执行顺序**

![导航守卫执行顺序](http://imgcache.gtimg.cn/vipstyle/game/act/lzh/images/201810231959_router.png)


[查看在线例子](https://jsfiddle.net/logan70/yo2a0thj/)

# 参考文章

[十全大补vue-router](https://juejin.im/post/5ab9a21ff265da238925bbf5)

[vue-router详解](https://www.jianshu.com/p/4c5c99abb864)

这两天学习vue-router的笔记，后续学习到的地方再做补充，有误的地方求大神指出