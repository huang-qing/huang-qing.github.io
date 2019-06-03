---
layout: post
title: React虚拟DOM的渲染过程和特性
subtitle:
date: 2019-05-29
author: huangqing
header-img: img/post-bg-react.png
catalog: true
categories: [React]
tags:
  - react
  - 虚拟DOM
---

React 的`虚拟DOM`和`Diff算法`是 React 的非常重要的核心特性。

开发中的常见问题:

- 为何必须引用 React
- 自定义的 React 组件为何必须大写
- React 如何防止 XSS
- React 的 Diff 算法和其他的 Diff 算法有何区别
- key 在 React 中的作用
- 如何写出高性能的 React 组件

## 虚拟DOM

![](/images/react/2019-05-29_153109.png)

在原生的 JavaScript程序中，我们直接对 DOM进行创建和更改，而 DOM元素通过我们监听的事件和我们的应用程序进行通讯。

而 React会先将你的代码转换成一个 JavaScript对象，然后这个 JavaScript对象再转换成真实 DOM。这个 JavaScript对象就是所谓的虚拟 DOM。

比如下面一段 html代码：

```html
<div class="title">
  <span>
    Hello ConardLi
  </span>
  <ul>
    <li>苹果</li>
    <li>橘子</li>
  </ul>
</div>
```

在 React可能存储为这样的 JS代码：

```javascript
const VitrualDom = {
  type: "div",
  props: { class: "title" },
  children: [
    {
      type: "span",
      children: "Hello"
    },
    {
      type: "ul",
      children: [
        { type: "li", children: "苹果" },
        { type: "li", children: "橘子" }
      ]
    }
  ]
}
```

当我们需要创建或更新元素时， React首先会让这个 `VitrualDom`对象进行创建和更改，然后再将 `VitrualDom`对象渲染成真实 `DOM`；

当我们需要对 `DOM`进行事件监听时，首先对 `VitrualDom`进行事件监听， `VitrualDom`会代理原生的 `DOM`事件从而做出响应。

## 为何使用虚拟DOM

#### 提高开发效率

使用 JavaScript，我们在编写应用程序时的关注点在于如何更新 DOM。

使用 React，你只需要告诉 React你想让视图处于什么状态， React则通过 VitrualDom确保 DOM与该状态相匹配。你不必自己去完成属性操作、事件处理、 DOM更新， React会替你完成这一切。

这让我们更关注我们的业务逻辑而非 DOM操作，这一点即可大大提升我们的开发效率。

#### 关于提升性能

直接操作 DOM是非常耗费性能的，这一点毋庸置疑。但是 React使用 VitrualDom也是无法避免操作 DOM的。

如果是首次渲染， VitrualDom不具有任何优势，甚至它要进行更多的计算，消耗更多的内存。

`VitrualDom的优势在于 React的 Diff算法和批处理策略`， React在页面更新之前，提前计算好了如何进行更新和渲染 DOM。

VitrualDom帮助我们提高了开发效率，在重复渲染时它帮助我们计算如何更高效的更新，而不是它比 DOM操作更快。

#### 跨浏览器兼容

![](/images/react/2019-05-29_163609.png)

React基于 VitrualDom自己实现了一套自己的事件机制，自己模拟了事件冒泡和捕获的过程，采用了`事件代理`，`批量更新`等方法，抹平了各个浏览器的事件兼容性问题。

### 跨平台兼容

VitrualDom为 React带来了跨平台渲染的能力。以 ReactNative为例子。`React根据 VitrualDom画出相应平台的 ui层`，只不过不同平台画的姿势不同而已。

## 虚拟DOM实现原理

![](/images/react/2019-05-29_164104.png)

在上面的图上我们继续进行扩展，按照图中的流程，我们依次来分析虚拟 DOM的实现原理。

我们在实现一个React组件时可以选择两种编码方式，第一种是使用`JSX`编写：
```javascript
class Hello extends Component {
  render() {
    return <div>Hello ConardLi</div>;
  }
}
```
第二种是直接使用`React.createElement`编写：
```javascript
class Hello extends Component {
  render() {
    return React.createElement('div', null, `Hello ConardLi`);
  }
}
```
实际上，上面两种写法是等价的，`JSX`只是为 `React.createElement(component, props, ...children)` 方法提供的语法糖。也就是说所有的JSX 代码最后都会转换成`React.createElement(...)` ，Babel帮助我们完成了这个转换的过程。

如下面的JSX
```html
<div>
  <img src="avatar.png" className="profile" />
  <Hello />
</div>
```
将会被`Babel`转换为
```javascript
React.createElement("div", null, React.createElement("img", {
  src: "avatar.png",
  className: "profile"
}), React.createElement(Hello, null));
```
注意，`babel`在编译时会判断JSX中组件的首字母，*当首字母为小写时，其被认定为原生DOM标签*，createElement的第一个变量被编译为字符串；*当首字母为大写时，其被认定为自定义组件，createElement的第一个变量被编译为对象*；

另外，由于JSX提前要被Babel编译，所以JSX是不能在运行时动态选择类型的，比如下面的代码：
```javascript
function Story(props) {
  // Wrong! JSX type can't be an expression.
  return <components[props.storyType] story={props.story} />;
}
```
需要变成下面的写法：
```javascript
function Story(props) {
  // Correct! JSX type can be a capitalized variable.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```
所以，使用JSX你需要安装`Babel`插件`babel-plugin-transform-react-jsx`
```javascript
{
    "plugins": [
        ["transform-react-jsx", {
            "pragma": "React.createElement"
        }]
    ]
}
```

## 创建虚拟DOM

下面我们来看看虚拟DOM的真实模样，将下面的JSX代码在控制台打印出来：
```javascript
<div className="title">
      <span>Hello ConardLi</span>
      <ul>
        <li>苹果</li>
        <li>橘子</li>
      </ul>
</div>
```
![](/images/react/diff2.png)

这个结构和我们上面自己描绘的结构很像，那么React是如何将我们的代码转换成这个结构的呢，下面我们来看看`createElement`函数的具体实现（文中的源码经过精简）。

![](/images/react/diff2.png)