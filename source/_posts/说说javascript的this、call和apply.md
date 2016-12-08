---
title: 说说javascript的this、call和apply
date: 2016-12-08 16:49:25
tags: [javascript]
categories: javascript核心
comments: true
---
### JavaScript核心-函数this、call和apply

#### 函数this的用法

his是Javascript语言的一个关键字。

它代表函数运行时，自动生成的一个内部对象，只能在函数内部使用。比如

```javascript
function test(){
　　this.x = 1;
}
```
随着函数使用场合的不同，this的值会发生变化。但是有一个总的原则，那就是this指的是，调用函数的那个对象。

##### 纯粹的函数调用

这是函数的最通常用法，属于全局性调用，因此this就代表全局对象Global

```javascript
function test(){
　　this.x = 1;
　　alert(this.x);
}
test(); // 1 此时的this是window
```
```javascript
var x = 1;
function test(){
　　alert(this.x);
}
test(); // 1
```

<!-- more -->


##### 作为对象方法的调用

函数还可以作为某个对象的方法调用，这时this就指这个上级对象。

```javascript
function test(){
　　alert(this.x);
}
var o = {};
o.x = 1;
o.m = test;
o.m(); // 1
```

##### 作为构造函数调用

所谓构造函数，就是通过这个函数生成一个新对象（object）。这时，this就指这个新对象。

```javascript
function test(){
　　this.x = 1;
}
var o = new test();
alert(o.x); // 1
```

```javascript
var x = 2;
function test(){
　　this.x = 1;
}
var o = new test();
alert(x); //2
alert(o.x); //1
```

#### 函数的arguments

arguments属性是正在执行的函数的内置属性，返回该函数的arguments对象。arguments对象包含了调用该函数时所传入的实际参数信息(参数个数、参数值等)。
**只有在当前函数正在执行时该属性才有效。**

##### 使用方法

arguments属性的值为Object类型，返回正在执行的当前函数的arguments对象。

arguments对象包含调用该函数时所传入的实际参数信息，例如：参数的个数和参数的值。我们可以通过arguments属性让函数处理可变数量的参数。

arguments对象有以下三个属性：

* length属性，返回实际传入的参数个数。
* callee属性，返回当前函数的引用(匿名函数可以使用该属性实现递归调用)。
* 0...n属性，以顺序索引访问传入的具体参数。例如，使用arguments[0]可以访问传入的第1个参数。

##### 示例

```javascript
function test(){
    document.writeln("实际传入的参数个数：" + arguments.length); // 实际传入的参数个数：3
    for(var i = 0; i < arguments.length; i++){
        document.writeln("传入的第" + (i + 1) +"个参数：" + arguments[i]);
    }
    // 传入的第1个参数：1 传入的第2个参数：张三 传入的第3个参数：true
    // callee属性返回的就是当前函数
    document.writeln( arguments.callee === test ); // true
};
test(1, "张三", true);

```
#### 函数的call

##### 使用方法

* call()函数用于调用当前函数，并可同时使用指定对象thisObj作为本次执行时函数内部的this指针引用。
* 就是说fn.call(a1),是fn函数中的this指向a1
* call()函数是将Function对象的参数一个个分别传入

##### 示例

```javascript
function foo(a, b){
    document.writeln(this.name);
    document.writeln(a);
    document.writeln(b);
}
// 改变this引用为obj，同时传递两个参数
foo.call(obj, 12, true); // 李四 12 true


function bar(a, b){
    var o = {name: "王五"};
    // 调用foo()函数，并改变其this为对象o，传入参数a,b作为其参数
    foo.call(o, a, b);
}
bar("CodePlayer", "www.365mini.com"); // 王五 CodePlayer www.365mini.com
```

#### 函数的apply

##### 使用方法

* 使用方法和作用与call()一样
* 注意apply()的参数必须是一个数组，或者arguments对象

##### 示例

```javascript
function foo(a, b){
    document.writeln(this.name);
    document.writeln(a);
    document.writeln(b);
}
// 改变this引用为obj，同时传递两个参数
foo.apply(obj, [12, true]); // 李四 12 true


function bar(){
    var o = {name: "王五"};
    // 调用foo()函数，并改变其this为对象o，传入当前函数的参数对象arguments作为其参数
    foo.apply(o, arguments);
}
bar("CodePlayer", "www.365mini.com"); // 王五 CodePlayer www.365mini.com
```

### 参考资料

* [javascript手册](http://www.365mini.com/page/javascript-function-apply.htm)
* [this作用域](http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html)