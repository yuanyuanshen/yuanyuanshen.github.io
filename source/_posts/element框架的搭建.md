---
title: element框架的搭建
date: 2016-12-13 16:05:20
tags: [vue npm webpack]
categories: vue学习篇
comments: true
---
### element框架搭建

从朋友那里听说了element，就想着写个小demo，顺道学习vue，但是木有想到搭建框架就遇到了一堆问题
，不过还好，看了官网大家提问的问题，找到了解决办法。

#### 安装

推荐使用 npm 的方式安装，它能更好地和webpack打包工具配合使用。
~~~
npm i element-ui -D
~~~

#### 项目结构

~~~
|- src/  --------------------- 项目源代码
    |- App.vue
    |- main.js  -------------- 入口文件
|- .babelrc  ----------------- babel 配置文件
|- index.html  --------------- HTML 模板
|- package.json  ------------- npm 配置文件
|- README.md  ---------------- 项目帮助文档
|- webpack.config.js  -------- webpack 配置文件
~~~

#### package.json

**注意**对package.json中的scripts的理解

一个由脚本命令组成的hash对象，他们在包不同的生命周期中被执行。key是生命周期事件，value是要运行的命令。
我们可以使用

~~~
npm run dev #webpack 的--inline --hot 起作用的原因就是配置了scrips
~~~

<!-- more -->


~~~
{
  "name": "element-starter",
  "description": "A Vue.js project",
  "author": "yi.shyang@ele.me",
  "private": true,
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --inline --hot --port 1314",
    "build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
  },
  "dependencies": {
    "element-ui": "^1.0.0",
    "vue": "^2.1.4"
  },
  "devDependencies": {
    "babel-core": "^6.0.0",
    "babel-loader": "^6.0.0",
    "babel-preset-es2015": "^6.13.2",
    "cross-env": "^1.0.6",
    "css-loader": "^0.23.1",
    "file-loader": "^0.8.5",
    "style-loader": "^0.13.1",
    "vue-loader": "^9.8.0",
    "webpack": "beta",
    "webpack-dev-server": "beta"
  }
}
~~~
#### webpack.config.js

~~~
var path = require('path')
var webpack = require('webpack')

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js'
  },
  module: {
    loaders: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.css$/,
        loader: 'style-loader!css-loader'
      },
      {
        test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
        loader: 'file-loader'
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?\S*)?$/,
        loader: 'file-loader',
        query: {
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  devServer: {
    historyApiFallback: true,
    noInfo: true
  },
  devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    })
  ])
}

~~~

#### main.js

```js
import Vue from 'vue'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
import App from './App.vue'

Vue.use(ElementUI)

new Vue({
  el: '#app',
  render: h => h(App)
})
```
#### App.vue

```js
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <el-button @click.native="startHacking">Let's do it</el-button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Use Vue 2.0 Today!'
    }
  },

  methods: {
    startHacking () {
      this.$notify({
        title: 'It Works',
        message: 'We have laid the groundwork for you. Now it\'s your time to build something epic!',
        duration: 6000
      })
    }
  }
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
}
</style>
```
#### index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>element-starter</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="dist/build.js"></script>
  </body>
</html>

```
#### 运行

```bash
$ npm run dev
```

#### 编译

```bash
$ npm run build
```

#### 遇到的问题 _vm._h is not a function

~~~
vue.runtime.common.js?d43f:519 [Vue warn]: Error when rendering component <ElButton>: warn @ vue.runtime.common.js?d43f:519Vue._render @ vue.runtime.common.js?d43f:2959(anonymous function) @ vue.runtime.common.js?d43f:2189get @ vue.runtime.common.js?d43f:1652Watcher @ vue.runtime.common.js?d43f:1644Vue._mount @ vue.runtime.common.js?d43f:2188Vue$2.$mount @ vue.runtime.common.js?d43f:5978init @ vue.runtime.common.js?d43f:2500createComponent @ vue.runtime.common.js?d43f:4052createElm @ vue.runtime.common.js?d43f:3995createChildren @ vue.runtime.common.js?d43f:4103createElm @ vue.runtime.common.js?d43f:4028patch @ vue.runtime.common.js?d43f:4446Vue._update @ vue.runtime.common.js?d43f:2215(anonymous function) @ vue.runtime.common.js?d43f:2189get @ vue.runtime.common.js?d43f:1652Watcher @ vue.runtime.common.js?d43f:1644Vue._mount @ vue.runtime.common.js?d43f:2188Vue$2.$mount @ vue.runtime.common.js?d43f:5978init @ vue.runtime.common.js?d43f:2500createComponent @ vue.runtime.common.js?d43f:4052createElm @ vue.runtime.common.js?d43f:3995patch @ vue.runtime.common.js?d43f:4483Vue._update @ vue.runtime.common.js?d43f:2215(anonymous function) @ vue.runtime.common.js?d43f:2189get @ vue.runtime.common.js?d43f:1652Watcher @ vue.runtime.common.js?d43f:1644Vue._mount @ vue.runtime.common.js?d43f:2188Vue$2.$mount @ vue.runtime.common.js?d43f:5978initRender @ vue.runtime.common.js?d43f:2916Vue._init @ vue.runtime.common.js?d43f:3296Vue$2 @ vue.runtime.common.js?d43f:3343(anonymous function) @ main.js?3479:8(anonymous function) @ build.js:1092__webpack_require__ @ build.js:658fn @ build.js:84(anonymous function) @ build.js:1734__webpack_require__ @ build.js:658(anonymous function) @ build.js:705(anonymous function) @ build.js:708
vue.runtime.common.js?d43f:2961 Uncaught TypeError: _vm._h is not a function
    at Proxy.render (eval at <anonymous> (http://localhost:1314/dist/build.js:1301:1), <anonymous>:6130:46)
    at VueComponent.Vue._render (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:2952:22)
    at VueComponent.eval (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:2189:21)
    at Watcher.get (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:1652:27)
    at new Watcher (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:1644:12)
    at VueComponent.Vue._mount (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:2188:19)
    at VueComponent.Vue$2.$mount (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:5978:15)
    at init (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:2500:11)
    at createComponent (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:4052:9)
    at createElm (eval at <anonymous> (http://localhost:1314/dist/build.js:820:1), <anonymous>:3995:9)
~~~

解决方法

Vue 2.1.5 将 _h 重命名为 _c，而 Element 目前发的版本都是用以前的 compiler 编译的，导致新版 runtime 无法运行 Element。目前的解决方案是锁定 Vue 的版本为 2.1.4

锁定vue相关版本

```
# 重新安装一下版本
"vue-template-compiler": "2.1.4"
"vue-loader": "10.0.0"
"vue": "2.1.4"
```

具体命令如下：

```
npm remove # 卸载某个版本
npm remove vue
npm remove vue-template-compiler
npm remove vue-loader
npm install vue@2.1.4 #安装指定版本
npm install vue-template-compiler@2.1.4
npm install vue-loader@10.0.0
```

### 参考资料

* [框架代码](https://github.com/yuanyuanshen/element-demo)
* [element官网](http://element.eleme.io/)
* [element的github地址](https://github.com/ElemeFE/element/issues)
* [package.json配置项的理解](https://github.com/ericdum/mujiang.info/issues/6/)

### 坚持

准备使用element做一个网站的前端，希望自己能坚持下去。。。
