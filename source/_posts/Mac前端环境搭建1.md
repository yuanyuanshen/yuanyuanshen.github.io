---
title: Mac前端环境搭建1
date: 2017-02-06 14:18:57
tags: [mac]
categories: mac使用
comments: true
---

### Mac前端环境搭建

虽然平时用ipad，但是换Mac OSX系统之后，如果用于娱乐可能不会觉得不适应，但是用于前端
开发，觉得需要适应的还很多，安装git、nvm、php等工具就费了老多时间，把遇到的坑和一些好的工具分享给大家。

#### Homebrew

##### 1.Homebrew是干嘛的

首先是Homebrew，按我的理解就是类似npm，npm是包管理工具，用来管理各种依赖包，而Homebrew是软件包管理工具。
可以在Mac中方便的安装软件或者卸载软件，如git、wget、zsh等

##### 2.为什么需要Homebrew

linux系统有个通病，软件包依赖，当前主流的两大发行版本都自带了解决方案，Red hat有yum，Ubuntu有apt-get
你用mac os，有第三方支持：Homebrew，Homebrew简称brew。

##### 3.Homebrew的安装

```
//打开终端
sudo ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
<!-- more -->

##### 4.Homebrew的常用命令

```shell
brew search 软件名，如brew search wget //搜索软件
brew install 软件名，如brew install wget//安装软件
brew remove 软件名，如brew remove wget//卸载软件
brew install git
```
[Homebrew官网](http://brew.sh/index_zh-cn.html)

#### Iterm2命令行工具

[iTerm好用之处](https://my.oschina.net/gabriel1215/blog/356935)
[iTerm的酷功能](https://www.zhihu.com/question/27447370)

#### 命令行扩展工具oh my Zsh

Oh My Zsh可以与iTerm一起使用来增强命令行功能

##### 使用brew安装Zsh

```
brew install zsh
```
可以进行主题配置等等
[zsh安装配置](http://yijiebuyi.com/blog/b9b5e1ebb719f22475c38c4819ab8151.html)


##### 安装nvm

这篇讲的很好，我就不细说了

[mac安装nvm](http://www.cnblogs.com/greenteaone/p/5065981.html)

拿到Mac的第一天除了熟悉mac的基本操作，就干了这点事，虽然还是不适应，但是相信会越用越熟练。



