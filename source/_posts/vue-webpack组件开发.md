---
title: vue+webpack组件开发
date: 2016-11-24 20:45:22
tags: [webpack,vue]
categories: vue学习篇
comments: true
---

#### 建立Vue组件

##### 创建组件

###### 项目目录结构

在src目录下建立components文件夹，并在其中建立app.vue文件

```bash
# 项目名称 vue-components-demo
vue-components-demo
└─webpack.config.js //配置文件
└─package.json
└─src               //文件入口
  └─components      //组件存放文件夹
    └─app.vue       //vue组件
  └─main.js         //主js文件
  └─index.html      //主html文件
└─node_modules      //项目的依赖所在的文件
└─dist              //webpack打包后生成的文件夹
  └─bundle.js
```
###### package.json

```bash
{
  "name": "vue-conponents-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.18.2",
    "babel-loader": "^6.2.8",
    "babel-plugin-transform-runtime": "^6.15.0",
    "babel-preset-es2015": "^6.18.0",
    "babel-preset-stage-0": "^6.16.0",
    "babel-runtime": "^6.18.0",
    "css-loader": "^0.26.0",
    "file-loader": "^0.9.0",
    "style-loader": "^0.13.1",
    "url-loader": "^0.5.7",
    "vue": "^2.0.1",
    "vue-hot-reload-api": "^2.0.6",
    "vue-html-loader": "^1.2.3",
    "vue-loader": "^9.7.0",
    "vue-style-loader": "^1.0.0",
    "vue-template-compiler": "^2.0.8",
    "webpack": "^1.13.3",
    "webpack-merge": "^0.17.0"
  }
```
###### 注意

配置文件中需要添加对.vue文件处理的语句，因为忘记添加所以后期找错误找了很久

<!-- more -->

```bash
loaders: [
     {test: /\.js$/,loader: 'babel',exclude: /node_modules/},
     //图片转换，小于8K自动转化为base64的编码
     {test: /\.(png|jpg|gif)$/,loader: 'url-loader?limit=8192'},
     //.vue 文件使用vue-loader处理
     {test: /\.vue$/,loader: 'vue-loader',exclude: /node_modules/}
]
```

###### html文件

```bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue Demo!!</title>
</head>
<body>
<div id='div1'>
    <app></app>
</div>
<script src='../dist/app.bundle.js'></script>
</body>
</html>
```

###### 编辑app.vue文件

```bash
<template>
    <div class="message">{{msg}}</div>
</template>
<script>
    export default {
        data () {
            return {
                msg: 'Hello from vue-loader'
            }
        }
    }
</script>
<style>
    .message{
        color: red;
        font-size: 36px;
        font-weight: blod;
    }
</style>
```
###### 编辑main.js文件

```bash
import Vue from 'vue'
import App from './components/app.vue'

new Vue({
    el:'#div1',
    components: {App}
});
```
##### 运行查看效果

```bash
$ webpack
```

##### webpack的hot-reload

前端自动刷新现在已经很常见了，即改变页面后，浏览器自动刷新，但是这个功能在我们做单页面应用时候会很不好用，所以，webpack支持hot-reload(热替换)，当我们修改模块时候页面不会刷新，会直接在页面中生效。

###### hot-reload的基础—webpack-dev-server

webpack-dev-server支持两种模式的自动刷新页面：

* iframe模式（页面嵌入一个iframe并在其中呈现页面的变化）
* inline模式（一个小型的webpack-dev-server客户端会作为入口文件打包，这个客户端会在后端代码改变的时候刷新页面）

###### iframe模式

使用iframe模式无需额外的配置，输入命令

```bash
$ webpack
$ webpack-dev-server --port 8088
```
访问http://loacalhost:8080/webpack-dev-server/

###### inline模式

```bash
$ webpack-dev-server --inline --hot --port 8088
```
启动服务器，在浏览器中打开 http://loacalhost:8088 就可以看到我们的页面，
此时修改app.vue中的css，以及html中的文字，都可以看到在浏览器中立马呈现。


#### 参考资料
* [webpack+vue组件开发](http://mrzhang123.github.io/2016/06/02/webpack-vue-3/)
