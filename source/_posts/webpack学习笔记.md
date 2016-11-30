---
title: webpack学习笔记
date: 2016-11-18 19:18:27
tags: [npm,webpack]
categories: webpack学习篇
comments: true
---
### webpack学习笔记

#### webpack介绍

* 一个打包工具
* 一个模块加载工具
* 各种资源都可以当成模块来处理
* [官方网站](http://webpack.github.io/)

#### webpack安装

##### 全局安装
```bash
$ npm install webpack -g #全局安装webpack
```
##### 项目中安装
```bash
$ cd <项目目录> # 进入项目目录
$ npm init  # 会自动生成一个package.json文件[描述项目依赖]
$ npm install webpack --save-dev #将webpack增加到package.json文件中
```
##### 解析package.json

```bash
# package.json
{
  "name": "muma-game",
  "version": "1.0.0",
  "description": "Muma game",
  "main": "game.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^1.13.3"
  }
}
```
#### webpack配置

每个项目下都必须配置有一个webpack.config.js，就是一个配置项，告诉 webpack 它需要做什么。

```bash
# webpack.config.js
var webpack = require('webpack');
var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');
module.exports = {
    //插件项
    plugins: [commonsPlugin],
    //页面入口文件配置
    entry: {
        index : './src/js/page/index.js'
    },
    //入口文件输出配置
    output: {
        path: 'dist/js/page',
        filename: '[name].js'
    },
    module: {
        //加载器配置 需要使用npm来加载
        loaders: [
            { test: /\.css$/, loader: 'style-loader!css-loader' },
            { test: /\.js$/, loader: 'jsx-loader?harmony' },
            { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
            { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
        ]
    },
    //其它解决方案配置
    resolve: {
        root: 'E:/github/flux-example/src', //绝对路径
        extensions: ['', '.js', '.json', '.scss'],
        alias: {
            AppStore : 'js/stores/AppStores.js',
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
        }
    }
};
```
<!-- more -->

#### webpack使用

##### 创建文件结构如下

源文件放在src中，把打包后的文件放在dist中。

```bash
project
└─webpack.config.js
└─package.json
└─src
  └─app.js
  └─game.js
└─node_modules
└─dist
  └─app.bundle.js
```
##### 创建package.json

```bash
$ npm init  # 会自动生成一个package.json文件[描述项目依赖]
```

##### 创建webpack.config.js

```bash
# app.js
cats = require('./game.js');
console.log(cats);
```

```bash
# game.js
var cats = ['dave', 'henry', 'martha'];
module.exports = cats;
```

```bash
# webpack.config.js
module.exports = {
     entry: './src/app.js',
     output: {
         path: './dist',
         filename: 'app.bundle.js'
     }
};
```

##### 在配置文件下，运行webpack

```bash
$ webpack
```

##### 运行 dist/app.bundle.js

```bash
$ node dist/app.bundle.js
# 结果 ["dave", "henry", "martha"]
```

#### webpack使用loader

**loaderLoader**可以理解为是模块和资源的转换器，它本身是一个函数，接受源文件作为参数，返回转换的结果。这样，我们就可以通过 require 来加载任何类型的模块或文件，比如 CoffeeScript、 JSX、 LESS 或图片。

##### 项目目录结构

```bash
project
└─webpack.config.js
└─package.json
└─src
  └─game.js
  └─app.js
  └─game.html
└─node_modules
└─dist
  └─app.bundle.js
└─css
  └─style.css
```
##### 安装loader

```bash
$ npm install style-loader css-loader
```

##### 修改相关文件

```bash
# style.css
body {
    background: yellow;
}
```

```bash
# app.js
require("../css/style.css");
document.write(require('./game.js'));
```

```bash
# game.js
var cats = ['dave', 'henry', 'martha'];
module.exports = cats;
```

```bash
# game.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script src="../dist/app.bundle.js"></script>
</body>
</html>
```

##### 修改配置文件

```bash
# webpack.config.js
module.exports = {
     entry: './src/app.js',
     output: {
         path: './dist',
         filename: 'app.bundle.js'
     },
     module: {
         loaders: [
             {test: /\.css$/,loader: "style!css"}
         ]
     }
};
```

##### 运行webpack命令

```bash
$ webpack #打开game.html可以看到黄色背景
```

#### webpack使用插件

插件的使用一般是在 webpack 的配置信息 plugins 选项中指定。Webpack 本身内置了一些常用的插件，还可以通过 npm 安装第三方插件。

##### BannerPlugin

我们利用一个最简单的 BannerPlugin 内置插件来实践插件的配置和运行，这个插件的作用是给输出的文件头部添加注释信息。

```bash
# webpack.config.js
var webpack = require('webpack')
module.exports = {
     entry: './src/app.js',
     output: {
         path: './dist',
         filename: 'app.bundle.js'
     },
     module: {
         loaders: [
             {test: /\.css$/,loader: "style!css"}
         ]
     },
    plugins: [
        new webpack.BannerPlugin('This file is created by zhaoda')
    ]
};
```

##### 运行webpack

```bash
$ webpack #打开app.bundle.js可以看到头部注释信息
```

#### webpack开发环境

**webpack-dev-server**开发服务是一个更好的选择。它将在 localhost:8080 启动一个 express 静态资源 web 服务器，并且会以监听模式自动运行 webpack，在浏览器打开 http://localhost:8080/ 或 http://localhost:8080/webpack-dev-server/ 可以浏览项目中的页面和编译后的资源输出，并且通过一个 socket.io 服务实时监听它们的变化并自动刷新页面。

```bash
$ npm install webpack-dev-server -g # 全局安装
$ webpack-dev-server --progress --colors
```

```bash
# 第二条命令输入完成报错如下，则说明端口被占用
events.js:141
      throw er; // Unhandled 'error' event
      ^

Error: listen EACCES 127.0.0.1:8080
    at Object.exports._errnoException (util.js:870:11)
    at exports._exceptionWithHostPort (util.js:893:20)
    at Server._listen2 (net.js:1218:19)
    at listen (net.js:1267:10)
    at net.js:1376:9
    at GetAddrInfoReqWrap.asyncCallback [as callback] (dns.js:63:16)
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:82:10)
```
**解决方法**

```bash
# 修改端口
$ webpack-dev-server --progress --colors --port 8088
```
**查看结果**

http://localhost:8088/webpack-dev-server/
服务实时监听它们的变化并自动刷新页面

**注意**

但这里可能会遇到，我们改动js文件，无法实时更新的问题，很大一部分原因是没有理解编译后的app.bundle.js是虚拟的问题，本地其实质是没有编译的，
所以我们不能用本地的路径去处理，
需要将html文件中script中src更改为http://localhost:8080/dist/app.bundle.js.在从新更改js文件，发现会自动更新了。

#### webpack学习心得

看了很多资料，总算对webpack有了初步的认识，后续慢慢研究更强大的功能。

#### 参考资料
* [官方文档](http://webpack.github.io/docs/webpack-dev-server.html)
* [webpack中文指南](http://webpackdoc.com/)
* [webpack介绍](http://www.jianshu.com/p/b95bbcfc590d)
* [webpack入门](http://blog.csdn.net/keliyxyz/article/details/51577905)
