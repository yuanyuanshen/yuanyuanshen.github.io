---
title: Vue.js学习笔记
date: 2016-11-22 20:11:32
tags: [npm,webpack,vue]
categories: vue学习篇
comments: true
---

#### 项目创建结构

##### 项目目录

```bash
# 项目名称 vue-simple-demo
# 目录如下
vue-simple-demo
└─webpack.config.js
└─package.json
└─src
  └─main.js
  └─index.html
└─node_modules
└─dist
  └─bundle.js
```

##### 项目文件

###### 项目目录下新建package.json

```bash
$ cd #当前目录
$ npm init #创建package.json
```
###### src目录下新建index.html

```bash
# 用于最终显示效果的html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue Demo</title>
</head>
<body>
<div id="div1">{{message}}</div>
<script src='../dist/bundle.js'></script>
</body>
</html>
</html>
```
<!-- more -->

###### sec目录下新建main.js

```bash
# main.js
import Vue from 'vue'
new Vue({
    el: '#div1',
    data: {
        message: 'vue demo'
    }
});
```
##### 安装依赖文件

###### 安装webpack，webpack-dev-server以及相关的loaders
```bash
$ npm install -g webpack
$ npm install -g webpack-dev-server
# 为项目安装其他依赖
$ npm i webpack-merge css-loader style-loader file-loader url-loader babel-core babel-loader babel-plugin-transform-runtime babel-preset-es2015 babel-preset-stage-0 babel-runtime vue vue-loader vue-html-loader vue-style-loader vue-hot-reload-api -D
```
**注意：** *vue是2.0.8版本 el属性不能为标签*

```bash
# package.json 文件
{
  "name": "vue-simple-demo",
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
    "vue": "^2.0.8",
    "vue-hot-reload-api": "^2.0.6",
    "vue-html-loader": "^1.2.3",
    "vue-loader": "^10.0.0",
    "vue-style-loader": "^1.0.0",
    "vue-template-compiler": "^2.0.8",
    "webpack": "^1.13.3",
    "webpack-merge": "^0.17.0"
  }
}
```
###### 相关的loaders说明

* webpack-merge：开发环境和生产环节的webpaak配置文件的配置合并
* file-loader：编译写入文件，默认情况下生成文件的文件名是文件名与MD5哈希值的组合
* vue-laoder：编译写入.vue文件
* vue-html-loader：编译vue的template部分
* vue-style-loader：编译vue的样式部分
* vue-hot-reload-api：webpack对vue实现热替换
* babel-core：ES2015编译核心
* babel-loader：编译写入ES2015文档
* babel-preset-es2015：ES2015语法
* babel-preset-stage-0：开启测试功能
* babel-runtime：babel执行环境

##### 配置webpack.config.js

```bash
var path = require('path');

module.exports = {
    entry: './src/main.js',
    output: {
        path: './dist',
        publicPath: 'dist/',
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            {test: /\.js$/,loader: 'babel',exclude: /node_modules/},
            //图片转换，小于8K自动转化为base64的编码
            {test: /\.(png|jpg|gif)$/,loader: 'url-loader?limit=8192'}
        ]
    },
    //这里用于安装babel，如果在更目录下的.babelrc配置了，这里可以不写
    babel: {
        presets: ['es2015','stage-0'],
        plugins: ['transform-runtime']
    }
}
```
###### 特别说明

如果要在.babelrc下配置babel，则需要在根目录下新建该文件，windows环境下，不能新建该txt文件然后改后缀，需要通过dos命令建立：

```dos
echo>.babelrc
```
通过该命令就可以建立babelde配置文件，用编辑器打开，修改里面的内容为：
```dos
{
  "presets": ["es2015", "stage-0"],
  "plugins": ["transform-runtime"]
}
```
##### 运行项目

```bash
$ webpack
```
###### 使用webpack-dev-server

```bash
# 修改index.html文件，目的是当修改js文件时webpack-dev-server服务能够监听并刷新页面
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue Demo!!</title>
</head>
<body>
<div id="div1">{{message}}</div>
<script src='http://localhost:1314/dist/app.bundle.js'></script>
</body>
</html>
```

```bash
# 执行以下命令 访问 http://localhost:1314/webpack-dev-server/
$ webpack
$ webpack-dev-server --progress --color --port 1314
# 修改js文件保存之后，可以发现页面自动更新修改内容
```

#### 学习心得

* [vue-simple-demo 代码地址](https://github.com/yuanyuanshen/vue-simple-demo)
* [vue-componets-demo 代码地址](https://github.com/yuanyuanshen/vue-componets-demo)
* 搜了很多关于vue.js的入门介绍，参考资料部分都是笔者觉得容易理解和上手的一些文章，共同努力~~

#### 参考资料
* [webpack+vue起步](http://mrzhang123.github.io/2016/05/31/webpack-vue-2/)
* [webpack中loader介绍](https://segmentfault.com/a/1190000005742111)
* [vue组件化开发初体验](https://segmentfault.com/a/1190000004060034)
* [ES6核心内容](http://www.jianshu.com/p/ebfeb687eb70)

