---
title: vue-router带你出坑
date: 2016-12-16 21:46:30
tags: [npm,webpack,vue,vue-router]
categories: vue学习篇
comments: true
---
## vue-router 带你出坑

最近学习vue，关于vue-router看了3遍，才开始有了一些自己的理解，而且，对于初学者，我认为
vue-router的官方手册顺序并不适用，所以把自己认为重点的和难以理解的，还有学习顺序进行调整，希望对其他
初学者有所帮助，嗯，共同进步~

### 配置路由模式

~~~
const router = new VueRouter({//创建路由实例
  mode: 'history',
  routes
})
~~~

配置路由模式:

* hash: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器。
* history: 依赖 HTML5 History API 和服务器配置。查看 HTML5 History 模式.
* abstract: 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式。

<!-- more -->

#### hash模式（默认）

> 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

#### history模式

> 通过history完成 URL 跳转而无须重新加载页面。

因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 http://oursite.com/user/id 就会返回 404，这就不好看了。

**注意**为了防止404错误，需要写notFound.html页面防止页面找不到发生错误。

```javascript
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '*', component: NotFoundComponent }
  ]
})
```
### 动态路由匹配

当前有路径/foo，当你/foo/XXX，不管XXX是什么，你需要让他都显示某个组件component：A，或者
说路由到某一个页面，就需要使用动态路由配置

**复制代码查看效果**[在线测试地址](http://jsfiddle.net/yyx990803/4xfa2f19/)

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<div id="app">
  <p>
    <router-link to="/user/foo">/user/foo</router-link>
    <router-link to="/user/bar">/user/bar</router-link>
    <router-link to="/user/aa">/user/aa</router-link>
    <router-link to="/user/bb">/user/bb</router-link>
  </p>
  <router-view></router-view>
</div>
```

```javascript
const User = {
  template: `<div>你的ID是 {{ $route.params.id }}</div>`
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})

const app = new Vue({ router }).$mount('#app')
```
### 嵌套路由

个人觉得和动态路由有点像，/foo下能有两个子路由/foo/a和/foo/b分别跳转A和B页面，这时就可以
使用嵌套路由。



### 编程式的导航

1. 就是说你在代码中可以控制路由，包含几个跳转函数。

* router.push(location) history中会有记录
* router.replace(location) history中不会有记录
* router.go(n) 在history记录中向前跳转几页或者向后几页

~~~
// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
~~~

2. location的值可以有一下几种类型：

* 'home'
* {path:'home'}
* { name: 'user', params: { userId: 123 }} // 命名路由，变成/user/123
* { path: 'register', query: { plan: 'private' }} // 带查询参数，变成 /register?plan=private

### 命名视图

一个视图叫router-view，通俗的说，就是一个页面可以有多个视图，一个视图对应一个组件。
一个路由，n个视图n个组件，相当于一次路由展示了多个组件。

```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```
```javascript
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
### 路由信息对象

> 一个 route object（路由信息对象） 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的 route records（路由记录）。

route object 出现在多个地方:

* 组件内的 this.$route 和 $route watcher 回调（监测变化处理）;
* router.match(location) 的返回值
* scrollBehavior 方法的参数
* 导航钩子的参数：

```javascript
router.beforeEach((to, from, next) => {
  // to 和 from 都是 路由信息对象,后面使用路由的钩子函数就容易理解了
})
```
**具体还有其他属性请自行去看官方文档**

### Router 构造配置

有几个重要的点容易出错

~~~
declare type RouteConfig = {
  path: string;
  component?: Component;
  name?: string; // for named routes (命名路由)
  components?: { [name: string]: Component }; // for named views (命名视图组件)
  redirect?: string | Location | Function;
  alias?: string | Array<string>;
  children?: Array<RouteConfig>; // for nested routes
  beforeEnter?: (to: Route, from: Route, next: Function) => void;
  meta?: any;
}
~~~

#### scrollBehavior

下属代码意思，浏览网页滚动中间位置，下一页，点击浏览器返回键，保持上一页浏览位置，使用
vue-router返回上一页，从浏览器顶部从新开始。
~~~
scrollBehavior: function (to, from, savedPosition) {
        return savedPosition || { x: 0, y: 0 }
    },
~~~

* savedPosition ：在使用正常浏览器返回前进，遵从浏览器属性，记录浏览位置
* { x: 0, y: 0 }：在使用vue-router路由的页面从顶端开始显示

### 参考文档

* [vue-router手册](http://router.vuejs.org/zh-cn/essentials/history-mode.html)