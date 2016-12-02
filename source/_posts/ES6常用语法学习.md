---
title: ES6常用语法学习
date: 2016-12-02 11:27:40
tags: [ES6,ES2015]
categories: ES6学习篇
comments: true
---
## ES6常用语法学习总结

每学一个知识点，都应该把重点总结出来，按照提纲对知识点，看看那个点还有问题
进行二次学习。最近在看ES6，每看一部分总结一点，回头再看哪里没有掌握。分享给大家~

### let和const命令

##### let命令

###### 不存在变量提升

```bash
console.log(ss); //undefined
console.log(xx); //报错
var ss = 'a';
let xx = 'b';
```

var存在变量提提升，实际执行

```bash
var ss;
console.log(ss); //undefined
console.log(xx); //报错
ss = 'a';
let xx = 'b';
```

###### 暂时性死区

ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。
凡是在声明之前就使用这些变量，就会报错。

###### 不允许重复声明

let不允许在相同作用域内，重复声明同一个变量。无论是使用var 或者 let

###### 块级作用域与函数声明

ES6引入了块级作用域，明确允许在块级作用域之中声明函数。
并且ES6规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。

<!-- more -->

##### const命令

依然存在以下属性

* 暂时性死区
* 不存在变量提升
* 块级作用域
* 不允许重复声明

新属性

* 后续不能进行赋值操作
* 一旦声明变量，就必须立即初始化

##### 顶层对象的属性

ES6为了改变这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。也就是说，从ES6开始，全局变量将逐步与顶层对象的属性脱钩。

```bash
var a = 1;
// 如果在Node的REPL环境，可以写成global.a
// 或者采用通用方法，写成this.a
window.a // 1
let b = 1;
window.b // undefined
```

### 变量的解构赋值

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
（个人理解就是对象、字符串、数组的取值和赋值），有需要看参考资料

### class, extends和super

```bash
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        console.log(this.type + ' says ' + say)
    }
}

let animal = new Animal()
animal.says('hello') //animal says hello

class Cat extends Animal {
    constructor(){
        super()
        this.type = 'cat'
    }
}

let cat = new Cat()
cat.says('hello') //cat says hello
```
代码解读：

* class定义了一个“类”，构造（constructor）方法。
* this关键字则代表实例对象。constructor内定义的方法和属性是实例对象自己的，而constructor外定义的方法和属性则是所有实例对象可以共享的。
* Class之间通过extends关键字实现继承
* super关键字，它指代父类的实例（即父类的this对象）。子类必须在constructor方法中调用super方法，否则新建实例时会报错。
* 子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。

### arrow function

```bash
function(x, y) {
    x++;
    y--;
    return x + y;
}
(x, y) => {x++; y--; return x+y}
```
**ES5与ES6对比**

ES5： function name(参数)    {运算表达式;return 表达式}

ES6：              (参数) => {运算表达式;return 表达式}

**箭头函数解决this指向问题**

```bash
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout( () => {
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}
 var animal = new Animal()
 animal.says('hi')  //animal says hi
```
当我们使用箭头函数时，函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，它的this是继承外面的，因此内部的this就是外层代码块的this。

### template string

这个东西也是非常有用，当我们要插入大段的html内容到文档中时，传统的写法非常麻烦，所以之前我们通常会引用一些模板工具库，比如mustache等等。

```bash
# ES5
$("#result").append(
  "There are <b>" + basket.count + "</b> " +
  "items in your basket, " +
  "<em>" + basket.onSale +
  "</em> are on sale!"
);
```

我们要用一堆的'+'号来连接文本与变量，而使用ES6的新特性模板字符串``后，我们可以直接这么来写：

```bash
# ES6
$("#result").append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```

用反引号（\）来标识起始，用${}`来引用变量，而且所有的空格和缩进都会被保留在输出之中.



## 参考资料

* [ECMAScript6入门](http://es6.ruanyifeng.com/)
* [ES6在线编辑器](http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=latest%2Creact%2Cstage-2&experimental=false&loose=false&spec=false&code=%5B1%2C2%2C3%5D.map(n%20%3D%3E%20n%20%2B%201)%3B&playground=true)
* [ES6核心语法](https://segmentfault.com/a/1190000004365693)