---
layout: post
title: 深入探究 Function & Object 鸡蛋问题
subtitle:
date: 2019-05-24
author: huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
  - javascript
  - 原型链
---

## 引言

这篇文章深入探究下 `Function.__proto__ === Function.prototype` 引起的鸡生蛋蛋生鸡问题，并在这个过程中深入了解 `Object.prototype`. `Function.prototype`. `function Object` . `function Function` 之间的关系。

## Object.prototype

`Object.prototype` 表示 `Object` 的原型对象，其 `[[Prototype]]` 属性是 `null`，访问器属性 `__proto__` 暴露了一个对象的内部 `[[Prototype]]` 。 `Object.prototype` 并不是通过 `Object` 函数创建的，为什么呢？看如下代码

```javascript
function Foo() {
  this.value = "foo";
}
let f = new Foo();
f.__proto__ === Foo.prototype;
// true
```

实例对象的 `__proto__`指向构造函数的 prototype，即 `f.__proto__` 指向 `Foo.prototype`，但是 `Object.prototype.__proto__` 是 `null`，所以 `Object.prototype` 并不是通过 `Object` 函数创建的，那它如何生成的?其实 `Object.prototype` 是浏览器底层根据 ECMAScript 规范创造的一个对象。

`Object.prototype` 就是原型链的顶端（不考虑 null 的情况下），所有对象继承了它的 `toString` 等方法和属性。

![](/images/javascript/2019-05-24_104143.png)

## Function.prototype

`Function.prototype` 对象是一个函数（对象），其 `[[Prototype]]` 内部属性值指向内建对象 `Object.prototype`。`Function.prototype` 对象自身没有 `valueOf` 属性，其从 `Object.prototype` 对象继承了`valueOf` 属性。

![](/images/javascript/2019-05-24_134549.png)

`Function.prototype` 的 `[[Class]]` 属性是 `Function`，所以这是一个函数，但又不大一样。为什么这么说呢？因为我们知道只有函数才有 `prototype` 属性，但并不是所有函数都有这个属性，因为 `Function.prototype` 这个函数就没有。

```javascript
Function.prototype;
// ƒ () { [native code] }

Function.prototype.prototype;
// undefined
```

当然你会发现下面这个函数也没有 prototype 属性。

```javascript
let fun = Function.prototype.bind();
// ƒ () { [native code] }

fun.prototype;
// undefined
```

为什么没有呢，我的理解是 `Function.prototype` 是引擎创建出来的函数，引擎认为不需要给这个函数对象添加 `prototype` 属性，不然 Function.prototype.prototype… 将无休无止并且没有存在的意义。

## function Object

`Object` 作为构造函数时，其 `[[Prototype]]` 内部属性值指向 `Function.prototype`，即

```javascript
Object.__proto__ === Function.prototype;
// true
```

![](/images/javascript/2019-05-24_135159.png)

使用`new Object()` 创建新对象时，这个新对象的 `[[Prototype]]` 内部属性指向构造函数的 `prototype` 属性，对应上图就是 `Object.prototype`。

![](/images/javascript/2019-05-24_135238.png)

当然也可以通过对象字面量等方式创建对象。

- 使用对象字面量创建的对象，其 `[[Prototype]]` 值是 `Object.prototype`。

- 使用数组字面量创建的对象，其 `[[Prototype]]` 值是 `Array.prototype`。

- 使用 `function f(){}` 函数创建的对象，其 `[[Prototype]]` 值是 `Function.prototype`。

- 使用 `new fun()` 创建的对象，其中 fun 是由 JavaScript 提供的内建构造器函数之一(Object, Function, Array, Boolean, Date, Number, String 等等），其 `[[Prototype]]` 值是 `fun.prototype`。

- 使用其他 JavaScript 构造器函数创建的对象，其 `[[Prototype]]` 值就是该构造器函数的 `prototype` 属性。

```javascript
let o = { a: 1 };
// 原型链: o ---> Object.prototype ---> null

let a = ["yo", "whadup", "?"];
// 原型链: a ---> Array.prototype ---> Object.prototype ---> null

function f() {
  return 2;
}
// 原型链: f ---> Function.prototype ---> Object.prototype ---> null

var fun = new Function();
// 原型链: fun ---> Function.prototype ---> Object.prototype ---> null

function Foo() {
  return {};
}
let foo = new Foo();
// 原型链: foo ---> Foo.prototype ---> Object.prototype ---> null
```

## function Function

`Function` 构造函数是一个函数对象，其 `[[Class]]` 属性是 `Function`。`Function` 的 `[[Prototype]]` 属性指向了 `Function.prototype`，即

```javascript
Function.__proto__ === Function.prototype;
// true
```

![](/images/javascript/2019-05-24_135654.png)

Function & Object 鸡蛋问题

我们看下面这段代码

```javascript
Object instanceof Function; // true
Function instanceof Object; // true

Object instanceof Object; // true
Function instanceof Function; // true
```

`Object` 构造函数继承了 `Function.prototype`，同时 `Function` 构造函数继承了 `Object.prototype`。这里就产生了 鸡和蛋 的问题。为什么会出现这种问题，因为 `Function.prototype` 和 `Function.__proto__` 都指向 `Function.prototype`。

```javascript
// Object instanceof Function     即
Object.__proto__ === Function.prototype; // true

// Function instanceof Object     即
Function.__proto__.__proto__ === Object.prototype; // true

// Object instanceof Object         即
Object.__proto__.__proto__ === Object.prototype; // true

// Function instanceof Function 即
Function.__proto__ === Function.prototype; // true
```

对于 `Function.__proto__ === Function.prototype` 这一现象有 2 种解释，争论点在于 `Function` 对象是不是由 `Function` 构造函数创建的一个实例？

解释 1. YES：按照 JavaScript 中“实例”的定义，a 是 b 的实例即 `a instanceof b` 为 true，默认判断条件就是 `b.prototype` 在 a 的原型链上。而 `Function instanceof Function` 为 true，本质上即 `Object.getPrototypeOf(Function) === Function.prototype`，正符合此定义。

解释 2. NO：`Function` 是 `built-in` 的对象，也就是并不存在“Function对象由Function构造函数创建”这样显然会造成鸡生蛋蛋生鸡的问题。实际上，当你直接写一个函数时（如 `function f() {}` 或` x => x`），也不存在调用 `Function `构造器，只有在显式调用 Function 构造器时（如 `new Function('x', 'return x')` ）才有。

这里偏向于第二种解释，即先有 `Function.prototype` 然后有的 `function Function()` ，所以就不存在鸡生蛋蛋生鸡问题了，把  `Function.__proto__` 指向 `Function.prototype` 是为了保证原型链的完整，让 `Function` 可以获取定义在 `Object.prototype` 上的方法。

最后给一个完整的图，看懂这张图原型就没问题了。

![](/images/javascript/javascript-object-layout.jpg)

## 内置类型构建过程

JavaScript 内置类型是浏览器内核自带的，浏览器底层对 JavaScript 的实现基于 C/C++，那么浏览器在初始化 JavaScript 环境时都发生了什么？

1. 用 C/C++ 构造内部数据结构创建一个 OP 即 (Object.prototype) 以及初始化其内部属性但不包括行为。

2. 用 C/C++ 构造内部数据结构创建一个 FP 即 (Function.prototype) 以及初始化其内部属性但不包括行为。

3. 将 FP 的 [[Prototype]] 指向 OP。

4. 用 C/C++ 构造内部数据结构创建各种内置引用类型。

5. 将各内置引用类型的[[Prototype]]指向 FP。

6. 将 Function 的 prototype 指向 FP。

7. 将 Object 的 prototype 指向 OP。

8. 用 Function 实例化出 OP，FP，以及 Object 的行为并挂载。

9. 用 Object 实例化出除 Object 以及 Function 的其他内置引用类型的 prototype 属性对象。

10. 用 Function 实例化出除Object 以及 Function 的其他内置引用类型的 prototype 属性对象的行为并挂载。

11. 实例化内置对象 Math 以及 Grobal

至此，所有内置类型构建完成。