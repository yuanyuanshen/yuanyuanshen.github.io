---
title: stylus常用语法总结
date: 2016-12-07 10:41:51
tags: [stylus,css]
categories: stylus学习篇
comments: true
---
## stylus常用语法总结

### stylus安装与使用

* 全局安装

```bash
$ npm install stylus -g
```
* .styl文件的编译

```bash
$ stylus -h #获得相关的命令行支持
$ stylus -w test.styl -o src #-w 是自动监视文件 -o 是将编译后的CSS文件输出到指定文件中
```

### stylus常用语法

#### 选择器（selector）

##### selector需要掌握要点

* 缩排 : 用缩进代替{},并且省略：当然用：分割属性和值也没有问题
* 逗号等于换行 : 逗号表达并列，stylus中新起一行表示并列意思
* 父级引用 : & 表示父级
* 消除歧义 ： 混入写法

<!-- more -->

##### selector要点举例

```css
# 编译前
html
body
  color white
  &:hover
    color red
    padding -5px
```

```css
# 编译后
html , body {
  color: #fff;
}
html:hover , body:hover {
  color: #fff;
  padding: -5px;
}
```
#### 变量(Variables)

##### Variables需要掌握要点

* 定义变量 : 指定表达式为变量，然后在样式中使用
* 属性查找 : 不需要定义变量直接引用属性

##### Variables要点举例

```css
# 编译前
font-size = 14px
body
  font font-size Arial, sans-seri
  div
    position absolute
    width 200px
    margin-left (@width/2)
```

```css
# 编译后
body {
    font：14px Arial, sans-seri;
}
body div {
    position:absolute;
    width: 200px;
    margin-left: 100px;
}

```
#### 插值(Interpolation)

##### Interpolation需要掌握要点

* 插值 : 通过使用{}字符包围表达式来插入值（有点像往css属性中插变量的意思）
* 选择器插值 : 不需要定义变量直接引用属性

##### Interpolation要点举例

```css
# 插值 编译前
vendor(prop, args)
  -webkit-{prop} args
  -moz-{prop} args
  {prop} args
border-radius()
  vendor('border-radius', arguments)
button
  border-radius 1px 2px / 3px 4px
```

```css
# 编译后
button {
  -webkit-border-radius: 1px 2px / 3px 4px;
  -moz-border-radius: 1px 2px / 3px 4px;
  border-radius: 1px 2px / 3px 4px;
}
```
#### 混合书写(Mixins)

```css
# 编译前
border-radius(n)
  -webkit-border-radius n
  -moz-border-radius n
  border-radius n
# 使用函数调用写法
form input[type=button]
  border-radius(5px)
# 把函数当做属性的写法，可以忽略括号
form input[type=button]
  border-radius 5px
```
```css
# 编译后
form input[type=button] {
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  border-radius: 5px;
}
```
#### 方法(Functions)

##### Functions需要掌握要点

* 参数设默认，返回多个值,别名 : 参数可以设置默认参数，一次返回可包含多个值
* 选择器插值 : 不需要定义变量直接引用属性
* 条件语句 : 函数中条件语句

##### Functions要点举例

```css
add(a b = 5px)
    a+b 20px # 省略的return
sdd = add
# 编译前
body
    padding sdd(10px)[0]
body
    padding sdd(10px)[1]
# 编译后
body {
  padding: 15px;
}
body {
  padding: 20px;
}
```
```css
compare(a, b)
  if a > b
    higher
  else if a < b
    lower
  else
    equal
# 使用
compare(5, 2) # higher

compare(1, 5) # lower

compare(10, 10) # equal
```
#### 内置方法(Built-in Functions)

##### 取颜色比重

* red()
* green()
* blue()
* alpha()

##### 颜色相关

* dark() 是否暗色
* light()是否亮色
* hue() 返回给定色调
* saturation() 返回给定饱和度

##### 取值运算

* abs() 取绝对值
* ceil() 向上取整
* floor() 向下取整
* round() 四舍五入
* max(a,b) min(a,b)取最大最小值
* even(unit) add(unit) 判断奇数偶数
* sum() avg() 求和求平均

##### 数组操作

* push(expr, args…) 返回expr+args
* unshift(expr, args…) 起始位置插入给定参数
* join(delim, vals…) 做连接

##### 键值运算

* keys(pairs) 返回pairs的键
* values(pairs) 返回pairs的值

##### 字符串匹配

* match(pattern, string) 检测string是否匹配给定的pattern

#### 注释(Comments)

* 单行注释 //
* 多行注释 /**/
* 多行缓冲注释 /*!*/ 压缩时无视这段直接输出

### 参考资料

[stylus中文文档](http://www.zhangxinxu.com/jq/stylus/selectors.php)
[stylus官网](http://stylus-lang.com/)