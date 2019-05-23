---
layout:     post
title:      你不知道的JavaScript 上
subtitle:   You Don't Know JS (1)
date:       2017-08-30
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
    - javascript
---


# You Don't Know JS

`作用域` `闭包` `this` `原型`

## 作用域

需要一套设计良好的规则来存储变量，并且之后可以方便地找到这些变量。这套规则被称为作用域。

作用域包括词法作用域和动态作用域。

作用域是一套规则，用于确定在何处以及如何查找变量（标识符）。

+ 如果查找的目的是*对变量进行赋值*，那么就会使用`LHS` 查询；
+ 如果目的是*获取变量的值*，就会使用`RHS` 查询。

赋值操作符会导致`LHS` 查询。`＝`操作符或调用函数时传入参数的操作都会导致关联作用域的赋值操作。

JavaScript 引擎首先会在代码执行前对其进行编译，在这个过程中，像`var a = 2` 这样的声明会被分解成两个独立的步骤：

1. 首先，var a 在其作用域中声明新变量。这会在最开始的阶段，也就是代码执行前进行。
2. 接下来，a = 2 会查询（LHS 查询）变量a 并对其进行赋值。

`LHS` 和`RHS` 查询都会在当前执行作用域中开始，如果有需要（也就是说它们没有找到所需的标识符），就会向上级作用域继续查找目标标识符，这样每次上升一级作用域（一层楼），最后抵达全局作用域（顶层），无论找到或没找到都将停止。

+ 不成功的`RHS` 引用会导致抛出`ReferenceError` 异常。
+ 不成功的`LHS` 引用会导致自动隐式地创建一个全局变量（非严格模式下），该变量使用`LHS` 引用的目标作为标识符，或者抛出`ReferenceError` 异常（严格模式下）。


词法作用域：

![作用域气泡](/images/javascript/scope-bubble.png)

1包含着整个全局作用域，其中只有一个标识符：`foo`。
2包含着`foo` 所创建的作用域，其中有三个标识符：`a`、`bar` 和`b`。
3包含着`bar` 所创建的作用域，其中只有一个标识符：`c`。

作用域查找会在找到第一个匹配的标识符时停止。在多层的嵌套作用域中可以定义同名的标识符，这叫作“遮蔽效应”（内部的标识符“遮蔽”了外部的标识符）。抛开遮蔽效应，作用域查找始终从运行时所处的最内部作用域开始，逐级向外或者说向上进行，直到遇见第一个匹配的标识符为止。

```javascript
function foo(a) {
    var b = a;
    return a + b;
}
var c = foo( 2 );

//1. 找出所有的LHS 查询（这里有3 处！）
//c = ..;、a = 2（隐式变量分配）、b = ..

//2. 找出所有的RHS 查询（这里有4 处！）
//foo(2..、= a;、a ..、.. b
```

JavaScript 中有两个机制可以“欺骗”词法作用域：`eval(..)` 和`with`。这两个机制的副作用是引擎无法在编译时对作用域查找进行优化，因为引擎只能谨慎地认为这样的优化是无效的。使用这其中任何一个机制都将导致代码运行变慢。**不要使用它们**。


### 函数作用域

函数作用域的含义是指，属于这个函数的全部变量都可以在整个函数的范围内使用及复用（事实上在嵌套的作用域中也可以使用）。这种设计方案是非常有用的，能充分利用JavaScript 变量可以根据需要改变值类型的“动态”特性。

隐藏内部实现：

```javascript
function doSomething(a) {
    function doSomethingElse(a) {
        return a - 1;
    }

    var b;
    b = a + doSomethingElse( a * 2 );
    console.log( b * 3 );
}

doSomething( 2 ); // 15
```

规避冲突:立即执行函数表达式

如果函数不需要函数名（或者至少函数名可以不污染所在作用域），并且能够自动运行，这将会更加理想。

`(function foo(){ .. })()`。第一个`()`将函数变成表达式，第二个`()`执行了这个函数。

`(function(){ .. }())`。仔细观察其中的区别。第一种形式中函数表达式被包含在`()`中，然后在后面用另一个`()`括号来调用。第二种形式中用来调用的`()`括号被移进了用来包装的`()`括号中。

这两种形式在功能上是一致的。选择哪个全凭个人喜好。

```javascript
(function foo(){ // <-- 添加这一行
    var a = 3;
    console.log( a ); // 3
})(); // <-- 以及这一行
```

匿名函数：

```javascript
setTimeout( function() {
    console.log("I waited 1 second!");
}, 1000 );
```

行内函数表达式:

非常强大且有用——匿名和具名之间的区别并不会对这点有任何影响。始终给函数表达式命名是一个最佳实践.

```javascript
setTimeout( function timeoutHandler() { // <-- 快看，我有名字了！
    console.log( "I waited 1 second!" );
}, 1000 );
```

### 块作用域

`with`:

它不仅是一个难于理解的结构，同时也是块作用域的一个例子（块作用域的一种形式），用`with` 从对象中创建出的作用域仅在`with` 声明中而非外部作用域中有效。

`try/catch`:

ES3 规范中规定`try/catch` 的`catch` 分句会创建一个块作用域，其中声明的变量仅在`catch` 内部有效。

`let`:

ES6 引入了新的`let` 关键字，提供了除`var` 以外的另一种变量声明方式。`let` 关键字可以将变量绑定到所在的任意作用域中（通常是`{ .. }` 内部）。换句话说，`let`为其声明的变量隐式地了所在的块作用域。

`const`:

ES6 还引入了`const`，同样可以用来创建块作用域变量，但其值是固定的（常量）。之后任何试图修改值的操作都会引起错误。

## 作用域闭包

包括变量和函数在内的所有声明都会在任何代码被执行前首先被处理。换句话说，先有蛋（声明）后有鸡（赋值）。

函数声明和变量声明都会被提升。但是一个值得注意的细节是*函数会首先被提升*，然后才是变量。

## this

`this` 关键字是JavaScript 中最复杂的机制之一。它是一个很特别的关键字，被自动定义在所有函数的作用域中，`this` 提供了一种更优雅的方式来隐式“传递”一个对象引用，因此可以将API 设计得更加简洁并且易于复用。

`this` 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调用时的各种条件。`this` 的绑定和函数声明的位置没有任何关系，只取决于*函数的调用方式*。

当一个函数被调用时，会创建一个活动记录（有时候也称为*执行上下文*）。这个记录会包含函数在哪里被调用（调用栈）、函数的调用方法、传入的参数等信息。`this` 就是记录的其中一个属性，会在函数执行的过程中用到。

`this` 既不指向函数自身也不指向函数的词法作用域，`this` 实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用。


#### 绑定规则：

1. 默认绑定:独立函数调用。可以把这条规则看作是无法应用其他规则时的默认规则。

   ```javascript
    function foo() {
        console.log( this.a );
    }

    var a = 2;

    // 函数调用时应用了this 的默认绑定，因此this 指向全局对象。
    // 如果使用严格模式（strict mode），那么全局对象将无法使用默认绑定，因此this 会绑定到undefined
    foo(); // 2
   ```

2. 隐式绑定:是调用位置是否有上下文对象，或者说是否被某个对象拥有或者包含。

   ```javascript
    function foo() {
        console.log( this.a );
    }
    var obj = {
        a: 2,
        foo: foo
    };
    obj.foo(); // 2
   ```

    对象属性引用链中只有最顶层会影响调用位置。

   ```javascript
    function foo() {
        console.log( this.a );
    }

    var obj2 = {
        a: 42,
        foo: foo
    };

    var obj1 = {
        a: 2,
        obj2: obj2
    };

    obj1.obj2.foo(); // 42
   ```

    隐式丢失：一个最常见的this 绑定问题就是被隐式绑定的函数会丢失绑定对象，也就是说它会应用默认绑定，从而把`this` 绑定到全局对象或者`undefined` 上，取决于是否是严格模式。

   ```javascript
    function foo() {
        console.log( this.a );
    }

    var obj = {
        a: 2,
        foo: foo
    };

    var bar = obj.foo; // 函数别名！
    var a = "oops, global"; // a 是全局对象的属性

    bar(); // "oops, global"
   ```

3. 显式绑定：`call(..)` 和`apply(..)`

  + 硬绑定

   ```javascript
    function foo() {
        console.log( this.a );
    }

    var obj = {
        a:2
    };
    var bar = function() {
        foo.call( obj );
    };

    bar(); // 2

    setTimeout( bar, 100 ); // 2
    // 硬绑定的bar 不可能再修改它的this
    bar.call( window ); // 2
   ```

    由于硬绑定是一种非常常用的模式，所以在ES5 中提供了内置的方法`Function.prototype.bind`

    ```javascript
    function foo(something) {
        console.log( this.a, something );
        return this.a + something;
    }

    var obj = {
        a:2
    };

    var bar = foo.bind( obj );
    var b = bar( 3 ); // 2 3
    ```
    
  + API调用的“上下文”

   ```javascript
    function foo(el) {
        console.log( el, this.id );
    }
    var obj = {
        id: "awesome"
    };
    // 调用foo(..) 时把this 绑定到obj
    [1, 2, 3].forEach( foo, obj );
    // 1 awesome 2 awesome 3 awesome
   ```

4. new绑定
  
    1. 创建（或者说构造）一个全新的对象。
    2. 这个新对象会被执行`[[原型]]` 连接。
    3. 这个新对象会绑定到函数调用的`this`。
    4. 如果函数没有返回其他对象，那么`new` 表达式中的函数调用会自动返回这个新对象。

#### 优先级

`new` new绑定 > `call() apply()` 显示绑定 > 隐式绑定 > 默认绑定

箭头函数并不是使用function 关键字定义的，而是使用被称为“胖箭头”的操作符 `=>` 定义的。箭头函数不使用`this` 的四种标准规则，而是根据外层（函数或者全局）作用域来决定`this`

### 小结

如果要判断一个运行中函数的`this` 绑定，就需要找到这个函数的直接调用位置。找到之后就可以顺序应用下面这四条规则来判断`this` 的绑定对象。

1. 由`new` 调用？绑定到新创建的对象。

2. 由`call` 或者`apply`（或者bind）调用？绑定到指定的对象。

3. 由上下文对象调用？绑定到那个上下文对象。

4. 默认：在严格模式下绑定到`undefined`，否则绑定到全局对象。

一定要注意，有些调用可能在无意中使用默认绑定规则。如果想“更安全”地忽略this 绑定，你可以使用一个DMZ 对象，比如`ø = Object.create(null)`，以保护全局对象。

ES6 中的箭头函数并不会使用四条标准的绑定规则，而是根据当前的词法作用域来决定`this`，具体来说，箭头函数会继承外层函数调用的`this` 绑定（无论`this` 绑定到什么）。这其实和ES6 之前代码中的`self = this` 机制一样。




## 对象

#### 对象定义

两种方式：声明（字面量）和构造

```javascript
var myObj={
    key:value
}
```

```javascript
var myObj=new Object();
myObj.key=value;
```

Javascript中共有六种主要类型：
+ string
+ number
+ boolean
+ null
+ undefined
+ object

Javacript中对象子类型（内置对象）：
+ String
+ Number
+ Boolean
+ Object
+ Function
+ Array
+ Date
+ RegExp
+ Error

```javascript
var strPrimitive = "I am a string";
typeof strPrimitive; // "string"
strPrimitive instanceof String; // false

var strObject = new String( "I am a string" );
typeof strObject; // "object"
strObject instanceof String; // true

// 在必要时语言会自动把字符串字面量转换成一个String 对象
// 检查sub-type 对象
Object.prototype.toString.call( strObject ); // [object String]
```

#### 内容访问

对象内容访问需要使用`.` 操作符或者`[]`操作符。`.a `语法通常被称为“属性访问”，`["a"]` 语法通常被称为“键访问”

#### 可计算属性名

ES6 增加了*可计算属性名*，可以在文字形式中使用`[]`包裹一个表达式来当作属性名：

```javascript
var prefix = "foo";

var myObject = {
[prefix + "bar"]:"hello",
[prefix + "baz"]: "world"
};

myObject["foobar"]; // hello
myObject["foobaz"]; // world
```
#### 复制

对于JSON 安全（也就是说可以被序列化为一个JSON 字符串并且可以根据这个字符串解析出一个结构和值完全一样的对象）的对象来说，有一种巧妙的复制方法：

```javascript
var newObj = JSON.parse( JSON.stringify( someObj ) );
```

#### `getOwnPropertyDescriptor`（属性描述符）:

从ES5 开始，所有的属性都具备了*属性描述符*:

```javascript
var myObject = {
    a:2
};

Object.getOwnPropertyDescriptor( myObject, "a" );
// {
// value: 2,
// writable: true,
// enumerable: true,
// configurable: true
// }
```

```javascript
Object.defineProperty( myObject, "a", {
    value: 2,
    writable: true,
    configurable: true,
    enumerable: true
} );
```

#### `preventExtensions`（禁止扩展）：

禁止一个对象添加新属性并且保留已有属性，使用`Object.preventExtensions(..)`.

```javascript
var myObject = {
    a:2
};
Object.preventExtensions( myObject );
```

#### `seal`（密封）：

`Object.seal(..)` 会创建一个“密封”的对象，这个方法实际上会在一个现有对象上调用`Object.preventExtensions(..)` 并把所有现有属性标记为`configurable:false`。

#### `freeze`（冻结）：

`Object.freeze(..)` 会创建一个冻结对象，这个方法实际上会在一个现有对象上调用`Object.seal(..)` 并把所有“数据访问”属性标记为`writable:false`，这样就无法修改它们的值。这个方法是你可以应用在对象上的级别最高的不可变性。

#### Getter和Setter:

```javascript
var myObject = {
    // 给 a 定义一个getter
    get a() {
    return this._a_;
    },
    // 给 a 定义一个setter
    set a(val) {
    this._a_ = val * 2;
    }
};
myObject.a = 2;
myObject.a; // 4
```

#### `hasOwnProperty`（存在性）

```javascript
var myObject = {
    a:2
};

//in 操作符会检查属性是否在对象及其[[Prototype]] 原型链中
("a" in myObject); // true
("b" in myObject); // false

//hasOwnProperty(..) 只会检查属性是否在myObject 对象中
myObject.hasOwnProperty( "a" ); // true
myObject.hasOwnProperty( "b" ); // false
```

#### `propertyIsEnumerable` `keys` `getOwnPropertyNames`:

`propertyIsEnumerable(..)` 会检查给定的属性名是否直接存在于对象中（而不是在原型链上）并且满足`enumerable:true`。

`Object.keys(..)` 会返回一个数组，包含所有可枚举属性

`Object.getOwnPropertyNames(..)`会返回一个数组，包含所有属性，无论它们是否可枚举。

```javascript
var myObject = { };
Object.defineProperty(myObject,"a",
// 让a 像普通属性一样可以枚举
{ enumerable: true, value: 2 });

Object.defineProperty(myObject,"b",
// 让b 不可枚举
{ enumerable: false, value: 3 });

myObject.propertyIsEnumerable( "a" ); // true
myObject.propertyIsEnumerable( "b" ); // false

Object.keys( myObject ); // ["a"]
Object.getOwnPropertyNames( myObject ); // ["a", "b"]
```

#### `for..in` 

`for..in` 循环可以用来遍历对象的可枚举属性列表

ES5 中增加了一些数组的辅助迭代器，包括`forEach(..)`、`every(..)` 和`some(..)`

#### `for..of`

ES6 增加了一种用来遍历数组的`for..of` 循环语法，直接遍历值而不是数组下标。

使用ES6 中的符号`Symbol.iterator` 来获取对象的`@@iterator` 内部属性

### 小结

+ JavaScript 中的对象有字面形式（比如`var a = { .. }`）和构造形式（比如`var a = new Array(..)`）。字面形式更常用，不过有时候构造形式可以提供更多选项。

+ 许多人都以为“JavaScript 中万物都是对象”，这是错误的。对象是6 个（或者是7 个，取决于你的观点）基础类型之一。对象有包括function 在内的子类型，不同子类型具有不同的行为，比如内部标签`[object Array]` 表示这是对象的子类型数组。

+ 对象就是`键/ 值`对的集合。可以通过`.propName` 或者`["propName"]` 语法来获取属性值。访问属性时，引擎实际上会调用内部的默认`[[Get]]` 操作（在设置属性值时是`[[Put]]`），`[[Get]]` 操作会检查对象本身是否包含这个属性，如果没找到的话还会查找`[[Prototype]]`链（参见第5 章）。

+ 属性的特性可以通过属性描述符来控制，比如`writable` 和`configurable`。此外，可以使用`Object.preventExtensions(..)`、`Object.seal(..)` 和`Object.freeze(..)` 来设置对象（及其属性）的不可变性级别。

+ 属性不一定包含值——它们可能是具备`getter/setter` 的“访问描述符”。此外，属性可以是可枚举或者不可枚举的，这决定了它们是否会出现在`for..in` 循环中。

+ 你可以使用ES6 的`for..of` 语法来遍历数据结构（数组、对象，等等）中的值，`for..of`会寻找内置或者自定义的`@@iterator` 对象并调用它的`next()` 方法来遍历数据值。

## 混合对象“类”

面向类的设计模式：实例化（instantiation）、继承（inheritance）和（相对）多态（polymorphism）。

类的机制：建造（实例）、构造函数、类的继承、多态、多重继承

#### 混入

由于JavaScript 不会自动实现Vehicle到Car 的复制行为，所以我们需要手动实现复制功能。这个功能在许多库和框架中被称为`extend(..)`，但是为了方便理解我们称之为`mixin(..)`。

显示混入：

```javascript
// 非常简单的mixin(..) 例子:
function mixin( sourceObj, targetObj ) {
    for (var key in sourceObj) {
        // 只会在不存在的情况下复制
        if (!(key in targetObj)) {
        targetObj[key] = sourceObj[key];
        }
    }
    return targetObj;
}

var Vehicle = {
    engines: 1,
    ignition: function() {
        console.log( "Turning on my engine." );
    },
    drive: function() {
        this.ignition();
        console.log( "Steering and moving forward!" );
    }
};

var Car = mixin( Vehicle, {
    wheels: 4,
    drive: function() {
        Vehicle.drive.call( this );
        console.log("Rolling on all " + this.wheels + " wheels!");
    }
} );

```

寄生继承（显式混入模式的一种变体）:首先我们复制一份Vehicle 父类（对象）的定义，然后混入子类（对象）的定义（如果需要的话保留到父类的特殊引用），然后用这个复合对象构建实例.

```javascript
// “传统的JavaScript 类”Vehicle
function Vehicle() {
    his.engines = 1;
}
Vehicle.prototype.ignition = function() {
    console.log( "Turning on my engine." );
};
Vehicle.prototype.drive = function() {
    this.ignition();
    console.log( "Steering and moving forward!" );
};

// “寄生类” Car
function Car() {
    // 首先，car 是一个Vehicle
    var car = new Vehicle();
    // 接着我们对car 进行定制
    car.wheels = 4;
    // 保存到Vehicle::drive() 的特殊引用
    var vehDrive = car.drive;
    // 重写Vehicle::drive()
    car.drive = function() {
        vehDrive.call( this );
        console.log(
        "Rolling on all " + this.wheels + " wheels!"
    );
    return car;
}

var myCar = new Car();
myCar.drive();
// 发动引擎。
// 手握方向盘！
// 全速前进！
```

隐式混入：

通过在构造函数调用或者方法调用中使用`Something.cool.call( this )`，我们实际上“借用”了函数`Something.cool()` 并在`Another` 的上下文中调用了它。虽然这类技术利用了`this` 的重新绑定功能，但是`Something.cool.call( this )` 仍然无法变成相对（而且更灵活的）引用，所以使用时千万要小心。通常来说，尽量避免使用这样的结构，以保证代码的整洁和可维护性。

```javascript
var Something = {
    cool: function() {
        this.greeting = "Hello World";
        this.count = this.count ? this.count + 1 : 1;
    }
};

Something.cool();
Something.greeting; // "Hello World"
Something.count; // 1

var Another = {
    cool: function() {
        // 隐式把Something 混入Another
        Something.cool.call( this );
    }
};

Another.cool();
Another.greeting; // "Hello World"
Another.count; // 1 （count 不是共享状态）
```

### 小结

+ 类是一种设计模式。许多语言提供了对于面向类软件设计的原生语法。JavaScript 也有类似的语法，但是和其他语言中的类完全不同。

+ 类意味着复制。

+ 传统的类被实例化时，它的行为会被复制到实例中。类被继承时，行为也会被复制到子类中。

+ 多态（在继承链的不同层次名称相同但是功能不同的函数）看起来似乎是从子类引用父类，但是本质上引用的其实是复制的结果。

+ JavaScript 并不会（像类那样）自动创建对象的副本。

+ 混入模式（无论显式还是隐式）可以用来模拟类的复制行为，但是通常会产生丑陋并且脆弱的语法，比如显式伪多态（`OtherObj.methodName.call(this, ...)`），这会让代码更加难懂并且难以维护。

+ 此外，显式混入实际上无法完全模拟类的复制行为，因为**对象（和函数！别忘了函数也是对象）只能复制引用，无法复制被引用的对象或者函数本身**。忽视这一点会导致许多问题。

+ 总地来说，在JavaScript 中模拟类是得不偿失的，虽然能解决当前的问题，但是可能会埋下更多的隐患。

## 原型 `[[Prototype]]` 链机制

```javascript
var anotherObject = {
    a:2
};
// 创建一个关联到anotherObject 的对象
var myObject = Object.create( anotherObject );
myObject.a; // 2
```

*继承*意味着复制操作，JavaScript（默认）并不会复制对象属性。相反，JavaScript 会在两个对象之间创建一个关联，这样一个对象就可以通过委托访问另一个对象的属性和函数。*委托*这个术语可以更加准确地描述JavaScript 中对象的关联机制。

在普通的函数调用前面加上`new` 关键字之后，就会把这个函数调用变成一个“构造函数调用”。实际上，`new` 会劫持所有普通函数并用构造对象的形式来调用它。

在JavaScript 中对于“构造函数”最准确的解释是，所有带new 的函数调用。函数不是构造函数，但是当且仅当使用`new` 时，函数调用会变成“构造函数调用”。

```javascript
function NothingSpecial() {
    console.log( "Don't mind me!" );
}

var a = new NothingSpecial();
// "Don't mind me!"
a; // {}
```

JavaScript 开发者绞尽脑汁想要模仿类的行为：

```javascript
function Foo(name) {
    this.name = name;
}
Foo.prototype.myName = function() {
    return this.name;
};
var a = new Foo( "a" );
var b = new Foo( "b" );
a.myName(); // "a"
b.myName(); // "b"
```

思考下面的代码：

```javascript
function Foo() { /* .. */ }
Foo.prototype = { /* .. */ }; // 创建一个新原型对象
var a1 = new Foo();
a1.constructor === Foo; // false!
a1.constructor === Object; // true!
```
a1 并没有`.constructor` 属性，所以它会委托`[[Prototype]]` 链上的`Foo.prototype`。但是这个对象也没有`.constructor` 属性（不过默认的`Foo.prototype` 对象有这个属性！），所以它会继续委托，这次会委托给委托链顶端的`Object.prototype`。这个对象有`.constructor` 属性，指向内置的`Object(..)` 函数。

`a1.constructor` 是一个非常不可靠并且不安全的引用。通常来说要尽量避免使用这些引用。

两种把Bar.prototype 关联到Foo.prototype 的方法：

```javascript
// ES6 之前需要抛弃默认的Bar.prototype
Bar.ptototype = Object.create( Foo.prototype );

// ES6 开始可以直接修改现有的Bar.prototype
Object.setPrototypeOf( Bar.prototype, Foo.prototype );
```

#### `Object.create(..)`

```javascript
var foo = {
    something: function() {
    console.log( "Tell me something good..." );
}
};
var bar = Object.create( foo );
bar.something(); // Tell me something good...
```


`Object.create(..)` 是在ES5 中新增的函数，所以在ES5 之前的环境中（比如旧IE）如果要支持这个功能的话就需要使用一段简单的`polyfill` 代码:

```javascript
if (!Object.create) {
    Object.create = function(o) {
        function F(){}
        F.prototype = o;
        return new F();
    };
}
```

这段polyfill 代码使用了一个一次性函数`F`，我们通过改写它的`.prototype` 属性使其指向想要关联的对象，然后再使用`new F()` 来构造一个新对象进行关联。

### 小结

+ 如果要访问对象中并不存在的一个属性，`[[Get]]` 操作就会查找对象内部`[[Prototype]]` 关联的对象。这个关联关系实际上定义了一条“原型链”（有点像嵌套的作用域链），在查找属性时会对它进行遍历。

+ 所有普通对象都有内置的`Object.prototype`，指向原型链的顶端（比如说全局作用域），如果在原型链中找不到指定的属性就会停止。`toString()`、`valueOf()` 和其他一些通用的功能都存在于`Object.prototype` 对象上，因此语言中所有的对象都可以使用它们。

+ 关联两个对象最常用的方法是使用`new` 关键词进行*函数调用*，创建一个关联其他对象的新对象。使用`new` 调用函数时会把新对象的`.prototype` 属性关联到“其他对象”。带`new` 的函数调用通常被称为“构造函数调用”，尽管它们实际上和传统面向类语言中的类构造函数不一样。

+ 虽然这些JavaScript 机制和传统面向类语言中的“类初始化”和“类继承”很相似，但是JavaScript 中的机制有一个核心区别，那就是*不会进行复制*，对象之间是通过内部的`[[Prototype]]` 链关联的。

+ 出于各种原因，以“继承”结尾的术语（包括“原型继承”）和其他面向对象的术语都无法帮助你理解JavaScript 的真实机制。*相比之下，“委托”是一个更合适的术语，因为对象之间的关系不是复制而是委托*。

## 行为委托

`[[Prototype]]` 机制就是指对象中的一个内部链接引用另一个对象。

如果在第一个对象上没有找到需要的属性或者方法引用，引擎就会继续在`[[Prototype]]`关联的对象上进行查找。同理，如果在后者中也没有找到需要的引用就会继续查找它的`[[Prototype]]`，以此类推。这一系列对象的链接被称为“原型链”。JavaScript 中这个机制的本质就是对象之间的关联关系。

#### 面向委托的设计

典型的（“原型”）*面向对象*风格：

```javascript
function Foo(who) {
    this.me = who;
}
Foo.prototype.identify = function() {
    return "I am " + this.me;
};

function Bar(who) {
    Foo.call( this, who );
}

Bar.prototype = Object.create( Foo.prototype );
Bar.prototype.speak = function() {
    alert( "Hello, " + this.identify() + "." );
};

var b1 = new Bar( "b1" );
var b2 = new Bar( "b2" );

b1.speak();
b2.speak();
```

使用*对象关联风格*来编写功能完全相同的代码：

```javascript
Foo = {
    init: function(who) {
        this.me = who;
    },
    identify: function() {
        return "I am " + this.me;
    }
};

Bar = Object.create( Foo );
Bar.speak = function() {
    alert( "Hello, " + this.identify() + "." );
};
var b1 = Object.create( Bar );
b1.init( "b1" );

var b2 = Object.create( Bar );
b2.init( "b2" );

b1.speak();
b2.speak();
```

类风格代码的思维模型强调实体以及实体间的关系：

![类风格-完整版](/images/javascript/2017-08-23_153705.png)

简化版：

![类风格-简化版](/images/javascript/2017-08-23_154007.png)


看对象关联风格代码的思维模型：

![关联风格](/images/javascript/2017-08-23_154250.png)


使用纯JavaScript 实现类风格的代码：

```javascript
// 父类
function Widget(width,height) {
    this.width = width || 50;
    this.height = height || 50;
    this.$elem = null;
}

Widget.prototype.render = function($where){
    if (this.$elem) {
        this.$elem.css( {
            width: this.width + "px",
            height: this.height + "px"
        } ).appendTo( $where );
    }
};

// 子类
function Button(width,height,label) {
    // 调用“super”构造函数
    Widget.call( this, width, height );
    this.label = label || "Default";
    this.$elem = $( "<button>" ).text( this.label );
}

// 让Button“继承”Widget
Button.prototype = Object.create( Widget.prototype );
// 重写render(..)
Button.prototype.render = function($where) {
    // “super”调用
    Widget.prototype.render.call( this, $where );
    this.$elem.click( this.onClick.bind( this ) );
};
Button.prototype.onClick = function(evt) {
    console.log( "Button '" + this.label + "' clicked!" );
};

$( document ).ready( function(){
    var $body = $( document.body );
    var btn1 = new Button( 125, 30, "Hello" );
    var btn2 = new Button( 150, 40, "World" );
    btn1.render( $body );
    btn2.render( $body );
} );
```

ES6的class语法糖:

```javascript
class Widget {
    constructor(width,height) {
        this.width = width || 50;
        this.height = height || 50;
        this.$elem = null;
    }

    render($where){
        if (this.$elem) {
            this.$elem.css( {
            width: this.width + "px",
            height: this.height + "px"
            } ).appendTo( $where );
        }
    }
}

class Button extends Widget {
    constructor(width,height,label) {
        super( width, height );
        this.label = label || "Default";
        this.$elem = $( "<button>" ).text( this.label );
    }
    render($where) {
        super( $where );
        this.$elem.click( this.onClick.bind( this ) );
    }
    onClick(evt) {
        console.log( "Button '" + this.label + "' clicked!" );
    }
}

$( document ).ready( function(){
    var $body = $( document.body );
    var btn1 = new Button( 125, 30, "Hello" );
    var btn2 = new Button( 150, 40, "World" );
    btn1.render( $body );
    btn2.render( $body );
} );
```

使用对象关联风格委托来更简单地实现Widget/Button：

```javascript
var Widget = {
    init: function(width,height){
        this.width = width || 50;
        this.height = height || 50;
        this.$elem = null;
    },
    insert: function($where){
        if (this.$elem) {
            this.$elem.css( {
            width: this.width + "px",
            height: this.height + "px"
            } ).appendTo( $where );
        }
    }
};

var Button = Object.create( Widget );

Button.setup = function(width,height,label){
    // 委托调用
    this.init( width, height );
    this.label = label || "Default";
    this.$elem = $( "<button>" ).text( this.label );
};
Button.build = function($where) {
// 委托调用
this.insert( $where );
    this.$elem.click( this.onClick.bind( this ) );
};
Button.onClick = function(evt) {
    console.log( "Button '" + this.label + "' clicked!" );
};

$( document ).ready( function(){
    var $body = $( document.body );
    var btn1 = Object.create( Button );
    btn1.setup( 125, 30, "Hello" );
    var btn2 = Object.create( Button );
    btn2.setup( 150, 40, "World" );
    btn1.build( $body );
    btn2.build( $body );
} );
```

### 小结

+ 在软件架构中你可以选择是否使用*类和继承*设计模式。大多数开发者理所当然地认为类是唯一（合适）的代码组织方式，但是本章中我们看到了另一种更少见但是更强大的设计模式：*行为委托*。

+ 行为委托认为对象之间是兄弟关系，互相委托，而不是父类和子类的关系。JavaScript 的[[Prototype]] 机制本质上就是行为委托机制。也就是说，我们可以选择在JavaScript 中努力实现类机制，也可以拥抱更自然的[[Prototype]] 委托机制。

+ 当你只用对象来设计代码时，不仅可以让语法更加简洁，而且可以让代码结构更加清晰。

+ 对象关联（对象之前互相关联）是一种编码风格，它倡导的是直接创建和关联对象，不把它们抽象成类。对象关联可以用基于[[Prototype]] 的行为委托非常自然地实现。