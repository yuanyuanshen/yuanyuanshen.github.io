---
title: 'github 上传error: src refspec master does not match any 解决方法'
date: 2016-10-25 20:40:33
tags: [git,github]
categories: git与github学习篇
comments: true
---

研究两天，终于搭建好Hexo，这是在github上发表的第一篇帖子，纪念一下~

### 本地文件上传github 报错error：src refspec master does not match any

``` bash
$ git push origin master
```

### 可能存在原因
1. 本地文件与github上的文件有冲突
2. 本地需要提交的文件中存在空文件
3. 本地的origin和remote origin/master 没有建立关联（我出错的原因）

### 解决方法

- 针对第三种情况导致的错误，重新建立本地和远端的连接

``` bash
$ git remote remove origin
$ git remote add origin git@github.com:XXX/XXX.github.io.git
$ git push origin master
```

<!-- more -->

- 针对第一、二种情况导致的错误

``` bash
$ touch README
$ git add README
$ git commit -m "change"
$ git push origin master
```
###### 针对第一、二种情况更详细的说明[点击](http://www.jianshu.com/p/8d26730386f3)

### 总结

导致错误的原因就是没有理解 origin 和 master 

1. clone 操作之后，Git 会自动将此远程仓库命名为 origin ，建立指针指向项目的指针master
2. origin 只相当于一个指针指向你 github 远端的地址（仓库名）/（分支名）origin/master 代表远程分支

详细请看[origin/master详细讲解](http://blog.csdn.net/abo8888882006/article/details/12375091)





