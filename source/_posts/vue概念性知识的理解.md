---
title: vue概念性知识的理解
date: 2017-01-14 14:22:02
tags: [npm,webpack,vue]
categories: vue学习篇
comments: true
---

## vue的理解

最近忙于写基于element框架的项目，对于vue也是直接上手，使用过程中跌跌撞撞，但是总归有收获。
记录下自己学习心得。坚持写博客~加油，时间长不更新博客，都怕自己不会了~~

### 认识vue

是渐进式的框架，只关注view视图，vue是数据驱动，所以不要操作DOM

vue除了包含主要的vue.js之外，还有生态库如vue-router、vue-resource等。一些相关库可以在官网找到

### vue的两个核心

#### 响应的数据绑定

当数据发生改变->视图会自动更新

利用Object.definedProperty中setter/getter代理数据，监控数据的操作

#### 组合的视图组件

ui页面映射为组件数

划分组件可维护、可重用、可测试

![组件](http://sandbox.runjs.cn/uploads/rs/410/cyvsaybm/mtu1da7r.png)

<!-- more -->

### 虚拟DOM

js运行的很快，但是大量的操作和更新DOM是很慢的，需要在数据更新之后重新渲染页面，这样就造成在没有改变数据的
地方也会重新的渲染DOM，造成资源浪费。

虚拟DOM：在内存中生成与真实DOM对应的对象，就是虚拟DOM

在数据改变是，通过对比old虚拟DOM和new虚拟DOM，只关注修改的部分，对真实的DOM节点只更新修改的部分。

### MVVM模式

M：Model数据模型
V：view视图模板
V-M：视图模型，实现M和V的联系 实现双向绑定，M和V是各自独立的

### vue代理data数据

每一个vue实例都会代理data对象里所有的属性，这些被代理的属性是响应式的，但是新添加的属性不具备
响应功能，改变后不会更新视图。以下是在codePen上写的在线例子，先添加的属性，改变后不会更新视图

http://codepen.io/shenyuanyuan/pen/dNXqEX/

### 声明式渲染

vue的双向数据绑定，就是一种声明式的渲染，你只需要按照规定，就可以显现绑定，而不需要关注是怎么实现的

#### 声明式渲染

直接调用方法，而不关注怎么实现

#### 命令式渲染

一步一步的执行，需要关注怎么实现

### 模板

#### html模板

1. 基于DOM模板，内容是一些html的标签

2. 插值

* 文本模板 
* 使用v-html来解析成htmlDOM
* 可以写表达式，但不能写语句

模板中尽量少的插入逻辑，如果需要逻辑计算，可以使用属性计算

#### template字符串模板

vue实例下有对应的属性template,模板会替换挂载元素，两者不能共存，template下只能有一根元素

#### 模板render函数

```js
render(createElement){
    //createElement函数的三个参数：标签名 数据对象 子元素
    return createElement(
        "ul",//标签名 数据对象 子元素
        {
            class : {//绑定class
                bg : true,//bg这个class的样式会被渲染
            },
            style : {//绑定样式
                fontSize : "20px",
            },
            attrs : {//添加自定义属性
                abc : "haha",
            },
            domProps : {//标签的属性
                innerHTML : "我是html",
            }
        },
        [
            createElement("li",1),
            createElement("li",1),
            createElement("li",1)
        ]
    );
}
```

### vue构建方式

开始觉得独立构建和运行时构建很难理解，但是当你发现你看完模板编译这块，构建方式就很容易理解了

#### 独立构建

独立构建包含模板编译器并支持 template 选项。 它也依赖于浏览器的接口的存在，所以你不能使用它来为服务器端渲染。

#### 运行时构建

运行时构建不包含模板编译器，因此不支持 template 选项，只能用 render 选项，但即使使用运行时构建，在单文件组件中也依然可以写模板，因为单文件组件的模板会在构建时预编译为 render 函数。

npm导出的包默认是运行时构建，如果要改为独立构建，webpack需配置如下代码

~~~
resolve: {
  alias: {
    'vue$': 'vue/dist/vue.common.js'
  }
}
~~~