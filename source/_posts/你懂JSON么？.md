---
title: 你懂JSON么？
date: 2017-01-23 14:42:04
tags: json
categories: 学富0.5车
comments: true
---
### 你懂JSON么？

前端常做的事就是和后台调接口，总会有人告诉你，转成JSON格式。。。可是有时候本来就是JSON了，为什么还要转JSON。
不知道我对JSON的理解对不对，不对的可以指正。

#### 什么是JSON

* JavaScript Object Notation 就是我们所说的JSON
* JSON 是存储和交换文本信息的语法。类似 XML。
* JSON 比 XML 更小、更快，更易解析。
* 你在js中写 var a = {"a":"1"},js会直接解析成js对象{a:"1"}

#### JSON的值可以有哪些

* 数字（整数或浮点数）
* 字符串（在双引号中）
* 逻辑值（true 或 false）
* 数组（在方括号中）
* 对象（在花括号中）
* null

<!-- more -->

#### JSON对象

~~~
{ "firstName":"John" , "lastName":"Doe" }
~~~

#### JSON数组(放在中括号内)

~~~
{
"employees": [//employees的值就是数组
{ "firstName":"John" , "lastName":"Doe" },
{ "firstName":"Anna" , "lastName":"Smith" },
{ "firstName":"Peter" , "lastName":"Jones" }
]
}
~~~

#### JSON 使用 JavaScript 语法

因为 JSON 使用 JavaScript 语法，所以无需额外的软件就能处理 JavaScript 中的 JSON。
通过 JavaScript，您可以创建一个对象数组，并像这样进行赋值：

~~~
var employees = [
{ "firstName":"Bill" , "lastName":"Gates" },
{ "firstName":"George" , "lastName":"Bush" },
{ "firstName":"Thomas" , "lastName": "Carter" }
];
employees[0].lastName;
~~~

**注意：按我的理解，上面代码虽然是JS语法，但是使用了JSON格式，如果最后要转成JSON和后台对接，如下：**

~~~
{"employees":
    [{ "firstName":"Bill" , "lastName":"Gates" },
    { "firstName":"George" , "lastName":"Bush" },
    { "firstName":"Thomas" , "lastName": "Carter" }]
}//可以使用js的eval函数解析成js对象
~~~

**这就是JSON的数组表达方式，如果构造成这样，就是JSON格式，不需要再转JSON了**






