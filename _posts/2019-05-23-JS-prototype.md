---
layout: post
title: JS 中原型和原型链
subtitle:
date: 2019-05-23
author: huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
  - javascript
  - 原型链
---

首先要搞明白几个概念：

函数（function）

函数对象(function object)

本地对象(native object)

内置对象(build-in object)

宿主对象(host object)

## 函数

```javascript
function foo() {}

var foo = function() {};
```

前者为函数声明，后者为函数表达式。`typeof foo` 的结果都是 `function`。

## 函数对象

函数就是对象,代表函数的对象就是函数对象

>官方定义， 在 `Javascript` 中,每一个函数实际上都是一个函数对象.`JavaScript` 代码中定义函数，或者调用 `Function` 创建函数时，最终都会以类似这样的形式调用 `Function` 函数:`var newFun = new Function(funArgs, funBody)`

其实也就是说，我们定义的函数，语法上，都称为函数对象，看我们如何去使用。如果我们单纯的把它当成一个函数使用，那么它就是函数，如果我们通过他来实例化出对象来使用，那么它就可以当成一个函数对象来使用，在面向对象的范畴里面，函数对象类似于类的概念。
```javascript
var foo = new function(){
}

typeof foo // object
```
或者
```javascript
function Foo (){
}

var foo = new Foo();

typeof foo // object
```
上面，我们所建立的对象

## 本地对象

>ECMA-262 把本地对象（native object）定义为“独立于宿主环境的 ECMAScript 实现提供的对象”。简单来说，本地对象就是 ECMA-262 定义的类（引用类型）。它们包括：
Object,Function,Array,String,Boolean,Number
Date,RegExp,Error,EvalError,RangeError,ReferenceError,SyntaxError,TypeError,URIError.

我们不能被他们起的名字是本地对象，就把他们理解成对象（虽然是事实上，它就是一个对象，因为 JS 中万物皆为对象），通过

```javascript
typeof(Object)

typeof(Array)

typeof(Date)

typeof(RegExp)

typeof(Math)
```
返回的结果都是 `function`

也就是说其实这些本地对象（类）是通过 `function` 建立起来的，
```javascript
function Object(){
}

function Array(){
}

...
```

可以看出 `Object` 原本就是一个函数，通过 `new Object()`之后实例化后，创建对象。类似于 JAVA 中的类。

## 内置对象

>ECMA-262 把内置对象（built-in object）定义为“由 ECMAScript 实现提供的、独立于宿主环境的所有对象，在 ECMAScript 程序开始执行时出现”。这意味着开发者不必明确实例化内置对象，它已被实例化了。ECMA-262 只定义了两个内置对象，即 Global 和 Math （它们也是本地对象，根据定义，每个内置对象都是本地对象）。

理清楚了这几个概念，有助于理解我们下面要讲述的原型和原型链。

## prototype

`prototype` 属性是每一个函数都具有的属性，但是不是一个对象都具有的属性。比如

```javascript
function Foo(){
}

var foo = new Foo()；
```

其中 `Foo` 中有 `prototype` 属性，而 `foo` 没有。但是 `foo` 中的隐含的`__proto__`属性指向 `Foo.prototype`。

```javascript
foo.__proto__ === Foo.prototype
```

为什么会存在 prototype 属性？

Javascript 里面所有的数据类型都是对象，为了使 JavaScript 实现面向对象的思想，就必须要能够实现‘继承’使所有的对象连接起来。而如何实现继承呢？JavaScript 采用了类似 C++，java 的方式，通过 new 的方式来实现实例。


## `__proto__`

`__proto__`属性是每一个对象以及函数都隐含的一个属性。对于每一个含有`__proto__`属性，他所指向的是创建他的构造函数的 prototype。原型链就是通过这个属性构件的。

如果要理清原型和原型链的关系，首先要明确一下几个概念：

1. JS 中的所有东西都是对象，函数也是对象, 而且是一种特殊的对象

2. JS 中所有的东西都由 Object 衍生而来, 即所有东西原型链的终点指向 Object.prototype

3. JS 对象都有一个隐藏的`__proto__`属性，他指向创建它的构造函数的原型，但是有一个例外，`Object.prototype.__proto__`指向的是 `null`。

4. JS 中构造函数和实例(对象)之间的微妙关系

构造函数通过定义 prototype 来约定其实例的规格, 再通过 new 来构造出实例,他们的作用就是生产对象.
```javascript
function Foo(){

}

var foo = new Foo();
```

`foo` 其实是通过 `Foo.prototype` 来生成实例的。

构造函数本身又是方法(Function)的实例, 因此也可以查到它的`__proto__`(原型链)
```javascript
function Foo(){

}
```
等价于
```javascript
var Foo= new Function()；
```
而 Function 实际上是
```javascript
function Function(){
    Native Code
}
```
也就是等价于
```javascript
var Function= new Function();
```
所以说 `Function` 是通过自己创建出来的。正常情况下对象的`__proto__`是指向创建它的构造函数的 `prototype` 的.所以 `Function` 的`__proto__`指向的 `Function.prototype`

`Object` 实际上也是通过 `Function` 创建出来的
```javascript
typeof(Object)//function
```
所以，
```javascript
function Object(){

    Native Code

}
```
等价于
```javascript
var Object = new Function();
```
那么 `Object` 的`__proto__`指向的是` Function.prototype`，也即是
```javascript
Object.__proto__ === Function.prototype //true
```
下面再来看 `Function.prototype` 的`__proto__`指向哪里

因为 JS 中所有的东西都是对象，那么，`Function.prototype` 也是对象，既然是对象，那么 `Function.prototype` 肯定是通过 `Object` 创建出来的，所以，
```javascript
Function.prototype.__proto__ === Object.prototype //true
```
综上所述，`Function` 和 `Object` 的原型以及原型链的关系可以归纳为下图。

![](/images/javascript/2019-05-23_095154.png)

对于单个的对象实例，如果通过 Object 创建，
```javascript
var obj = new Object();
```
那么它的原型和原型链的关系如下图。

![](/images/javascript/2019-05-23_095338.png)


如果通过函数对象来创建，
```javascript
function Foo(){

}

var foo = new Foo();
```
那么它的原型和原型链的关系如下图

![](/images/javascript/2019-05-23_095406.png)

那 JavaScript 的整体的原型和原型链中的关系就很清晰了，如下图所示

![](/images/javascript/2019-05-23_095429.png)

