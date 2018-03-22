---
layout:     post
title:      JSON.parse() 和 JSON.stringify() – 高级用法
subtitle:   
date:       2018-03-16
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [javascript]
tags:
    - javascript
---


`JSON`对象的两个方法：`JSON.parse()` 和 `JSON.stringify()` 通常用做`JSON`对象和字符串之间的相互转换，这里不再详细介绍，你可以查看 [这篇文章](http://www.css88.com/archives/3919) 。

这里介绍一下，我主要介绍一下 `JSON.parse()` 和 `JSON.stringify()` 的高级用法，可以在实际应用中给我们带来一些方便。

## JSON.parse()

`JSON.parse()` 可以接受第二个参数，它可以在返回之前转换对象值。比如这例子中，将返回对象的属性值大写：

```javascript
const user = {
  name: 'John',
  email: 'john@awesome.com',
  plan: 'Pro'
};
 
const userStr = JSON.stringify(user);
 
const newUserStr = JSON.parse(userStr, (key, value) => {
  if (typeof value === 'string') {
    return value.toUpperCase();
  }
  return value;
});
 
console.log(newUserStr); //{name: "JOHN", email: "JOHN@AWESOME.COM", plan: "PRO"} 
```

注：尾随逗号在JSON 中无效，所以如果传递给它的字符串有尾随逗号，`JSON.parse()`将会抛出错误。

### JSON.stringify()

`JSON.stringify()` 可以带两个额外的参数，第一个是`替换函数`，第二个`间隔字符串`，用作隔开返回字符串。

参数：

+ `value` ： 将要转为JSON字符串的javascript对象。
+ `replacer` ：该参数可以是多种类型,如果是一个函数,则它可以改变一个javascript对象在字符串化过程中的行为, 如果是一个包含 String 和 Number 对象的数组,则它将作为一个白名单.只有那些键存在域该白名单中的键值对才会被包含进最终生成的JSON字符串中.如果该参数值为null或者被省略,则所有的键值对都会被包含进最终生成的JSON字符串中。
+ `space` ：该参数可以是一个 `String` 或` Number` 对象,作用是为了在输出的`JSON`字符串中插入空白符来增强可读性. 如果是`Number`对象, 则表示用多少个空格来作为空白符; 最大可为10,大于10的数值也取10.最小可为1,小于1的数值无效,则不会显示空白符. 如果是个` String`对象, 则该字符串本身会作为空白符,字符串最长可为10个字符.超过的话会截取前十个字符. 如果该参数被省略 (或者为null), 则不会显示空白符

替换函数可以用来过滤值，因为任何返回` undefined` 的值将不在返回的字符串中：

```javascript
const user = {
  id: 229,
  name: 'John',
  email: 'john@awesome.com'
};
 
function replacer(key, value) {
  console.log(typeof value);
  if (key === 'email') {
    return undefined;
  }
  return value;
}
 
const userStr = JSON.stringify(user, replacer);
// "{"id":229,"name":"John"}"
```

传入一个间隔参数的示例：

```javascript
const user = {
  name: 'John',
  email: 'john@awesome.com',
  plan: 'Pro'
};
 
const userStr = JSON.stringify(user, null, '...');
// "{
// ..."name": "John",
// ..."email": "john@awesome.com",
// ..."plan": "Pro"
// }"
```

## toJSON方法

如果一个被序列化的对象拥有 `toJSON` 方法，那么该 `toJSON` 方法就会覆盖该对象默认的序列化行为：不是那个对象被序列化，而是调用 `toJSON` 方法后的返回值会被序列化

```javascript
var obj = {
    foo: 'foo',
    toJSON:function(){
        return 'bar';
    }
}
JSON.stringify(obj);//'"bar"'
JSON.stringify({x:obj});//'{"x":"bar"}'
```

利用`toJSON`方法,我们可以修改对象转换成`JSON`的默认行为。

## 用 JSON.stringify 来格式化对象

在实际使用中,我们可能会格式化一些复杂的对象，这些对象往往对象内嵌套对象。直接看起来并不那么直观,结合上面的的 `replacer` 和 `space` 参数,我们可以这样格式化复杂对象：

```javascript
var censor = function(key,value){
    if(typeof(value) == 'function'){
         return Function.prototype.toString.call(value)
    }
    return value;
}
var foo = {bar:"1",baz:3,o:{name:'xiaoli',age:21,info:{sex:'男',getSex:function(){return 'sex';}}}};

console.log(JSON.stringify(foo,censor,4))
```

实际返回的字符串，记住是字符串，如下:

```javascript
{
    "bar": "1",
    "baz": 3,
    "o": {
        "name": "xiaoli",
        "age": 21,
        "info": {
            "sex": "男",
            "getSex": "function (){return 'sex';}"
        }
    }
}
```