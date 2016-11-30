---
title: 关于git中仓库与分支的理解
date: 2016-11-17 21:18:27
tags: [git,github]
categories: git与github学习篇
comments: true
---
## 关于git中仓库与分支的理解

使用不同的电脑更新hexo搭建的博客时，[Hexo搭建博客](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)中提到了使用分支，使不同电脑上也可以快速修改博客
大致意思就是使用hexo分支提交网站文件，使用master分支提交静态网页，觉得分支理解不够，所以重新总结。

#### 理解仓库与分支

##### 仓库
* 远程仓库：github上
* 本地仓库：本地电脑上

##### 分支

分支每个仓库下含有一个或者多个分支，本地仓库和远程仓库对应的。

##### 本地两个分支 master master2

master[本地仓库master分支] == origin/master[远程仓库master分支]
master2[本地仓库master2分支] == origin/master2[远程仓库master2分支]

##### 本地有一个分支master
master[本地仓库] == origin/master[远程仓库]

#### 创建分支并提交

```bash
 $ git checkout -b master2 //创建master2分支并切换
 $ git add .
 $ git commit -m "注释"
 $ git push origin master2 //将分支提交远程仓库
```

<!-- more -->

#### 设置默认分支

将两个分支都提交到github上，可以通过拉菜单选择你想要设置为默认分支的那个分支。

#### 删除分支

```bash
 $ git branch -d <本地分支>
 $ git push origin :<远程分支>
```

#### 问题总结

* 本地仓库名是否可以与github上仓库名不一致

    答：可以，仓库关联的方式有两种

    （1）从github仓库克隆出新的仓库。
    （2）把一个已有的本地仓库与github仓库关联并把本地仓库的内容推送到GitHub仓库。

    使用（2）方式，即使本地仓库名与github仓库名不同，也同样可以关联

* 本地创建新的仓库关联远程仓库

    答：本地新建文件夹，完成commit操作之后，本地仓库是master，远程仓库origin/master

    ```bash
    $ git init //初始化git
    $ git remote add origin XX //关联本地仓库与远程仓库
    $ git push -u origin master //本地仓库代码提交远程仓库
    ```
#### 相关资料

* [git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013752340242354807e192f02a44359908df8a5643103a000)，其中有对git使用方法的详细说明。
* [创建两个分支搭建Hexo](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)
* [关于分支的操作](http://blog.sina.com.cn/s/blog_7101508c010147s6.html)，比较详细和全面


