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

React 的`虚拟DOM`和`Diff算法`是 React 的非常重要的核心特性。[原文链接](https://segmentfault.com/a/1190000018891454?utm_source=tag-newest)

开发中的常见问题:

- 为何必须引用 React
- 自定义的 React 组件为何必须大写
- React 如何防止 XSS
- React 的 Diff 算法和其他的 Diff 算法有何区别
- key 在 React 中的作用
- 如何写出高性能的 React 组件

## 虚拟 DOM

![](/images/react/2019-05-29_153109.png)

在原生的 JavaScript 程序中，我们直接对 DOM 进行创建和更改，而 DOM 元素通过我们监听的事件和我们的应用程序进行通讯。

而 React 会先将你的代码转换成一个 JavaScript 对象，然后这个 JavaScript 对象再转换成真实 DOM。这个 JavaScript 对象就是所谓的虚拟 DOM。

比如下面一段 html 代码：

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

在 React 可能存储为这样的 JS 代码：

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
};
```

当我们需要创建或更新元素时， React 首先会让这个 `VitrualDom`对象进行创建和更改，然后再将 `VitrualDom`对象渲染成真实 `DOM`；

当我们需要对 `DOM`进行事件监听时，首先对 `VitrualDom`进行事件监听， `VitrualDom`会代理原生的 `DOM`事件从而做出响应。

## 为何使用虚拟 DOM

#### 提高开发效率

使用 JavaScript，我们在编写应用程序时的关注点在于如何更新 DOM。

使用 React，你只需要告诉 React 你想让视图处于什么状态， React 则通过 VitrualDom 确保 DOM 与该状态相匹配。你不必自己去完成属性操作、事件处理、 DOM 更新， React 会替你完成这一切。

这让我们更关注我们的业务逻辑而非 DOM 操作，这一点即可大大提升我们的开发效率。

#### 关于提升性能

直接操作 DOM 是非常耗费性能的，这一点毋庸置疑。但是 React 使用 VitrualDom 也是无法避免操作 DOM 的。

如果是首次渲染， VitrualDom 不具有任何优势，甚至它要进行更多的计算，消耗更多的内存。

`VitrualDom的优势在于 React的 Diff算法和批处理策略`， React 在页面更新之前，提前计算好了如何进行更新和渲染 DOM。

VitrualDom 帮助我们提高了开发效率，在重复渲染时它帮助我们计算如何更高效的更新，而不是它比 DOM 操作更快。

#### 跨浏览器兼容

![](/images/react/2019-05-29_163609.png)

React 基于 VitrualDom 自己实现了一套自己的事件机制，自己模拟了事件冒泡和捕获的过程，采用了`事件代理`，`批量更新`等方法，抹平了各个浏览器的事件兼容性问题。

### 跨平台兼容

VitrualDom 为 React 带来了跨平台渲染的能力。以 ReactNative 为例子。`React根据 VitrualDom画出相应平台的 ui层`，只不过不同平台画的姿势不同而已。

## 虚拟 DOM 实现原理

![](/images/react/2019-05-29_164104.png)

在上面的图上我们继续进行扩展，按照图中的流程，我们依次来分析虚拟 DOM 的实现原理。

我们在实现一个 React 组件时可以选择两种编码方式，第一种是使用`JSX`编写：

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
    return React.createElement("div", null, `Hello ConardLi`);
  }
}
```

实际上，上面两种写法是等价的，`JSX`只是为 `React.createElement(component, props, ...children)` 方法提供的语法糖。也就是说所有的 JSX 代码最后都会转换成`React.createElement(...)` ，Babel 帮助我们完成了这个转换的过程。

如下面的 JSX

```html
<div>
  <img src="avatar.png" className="profile" />
  <Hello />
</div>
```

将会被`Babel`转换为

```javascript
React.createElement(
  "div",
  null,
  React.createElement("img", {
    src: "avatar.png",
    className: "profile"
  }),
  React.createElement(Hello, null)
);
```

注意，`babel`在编译时会判断 JSX 中组件的首字母，_当首字母为小写时，其被认定为原生 DOM 标签_，createElement 的第一个变量被编译为字符串；_当首字母为大写时，其被认定为自定义组件，createElement 的第一个变量被编译为对象_；

另外，由于 JSX 提前要被 Babel 编译，所以 JSX 是不能在运行时动态选择类型的，比如下面的代码：

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

所以，使用 JSX 你需要安装`Babel`插件`babel-plugin-transform-react-jsx`

```javascript
{
    "plugins": [
        ["transform-react-jsx", {
            "pragma": "React.createElement"
        }]
    ]
}
```

## 创建虚拟 DOM

下面我们来看看虚拟 DOM 的真实模样，将下面的 JSX 代码在控制台打印出来：

```html
<div className="title">
  <span>Hello ConardLi</span>
  <ul>
    <li>苹果</li>
    <li>橘子</li>
  </ul>
</div>
```

![](/images/react/diff2.png)

#### ReactElement

`ReactElement`将传入的几个属性进行组合，并返回。

- `type`：元素的类型，可以是原生`html`类型（字符串），或者自定义组件（函数或`class`）
- `key`：组件的唯一标识，用于`Diff`算法，下面会详细介绍
- `ref`：用于访问原生`dom`节点
- `props`：传入组件的`props`
- `owner`：当前正在构建的 Component 所属的`Component`
- `$$typeof`：一个我们不常见到的属性，它被赋值为`REACT_ELEMENT_TYPE`：

```js
var REACT_ELEMENT_TYPE =
  (typeof Symbol === "function" && Symbol.for && Symbol.for("react.element")) ||
  0xeac7;
```

可见，`$$typeof`是一个`Symbol`类型的变量，这个变量可以防止`XSS`。

## 虚拟 DOM 转换为真实 DOM

用流程图梳理一下整个过程，整个过程大概可分为四步：

![](/images/react/diff4.png)

## 虚拟 DOM 原理、特性总结

#### React 组件的渲染流程

- 使用`React.createElement` 或 `JSX` 编写 `React` 组件，实际上所有的 `JSX` 代码最后都会转换成 `React.createElement(...)` ，`Babel` 帮助我们完成了这个转换的过程。
- `createElement` 函数对 `key` 和 `ref` 等特殊的 `props` 进行处理，并获取 `defaultProps` 对默认 `props` 进行赋值，并且对传入的孩子节点进行处理，最终构造成一个 `ReactElement` 对象（所谓的虚拟 DOM）。
- `ReactDOM.render` 将生成好的虚拟 `DOM` 渲染到指定容器上，其中采用了批处理、事务等机制并且对特定浏览器进行了性能优化，最终转换为真实 `DOM`。

#### 虚拟 DOM 的组成

即 `ReactElementelement` 对象，我们的组件最终会被渲染成下面的结构：

- `type`：元素的类型，可以是原生 `html` 类型（字符串），或者`自定义组件`（函数或 `class`）
- `key`：组件的`唯一标识`，用于 `Diff` 算法
- `ref`：用于访问原生`dom` 节点
- `props`：传入组件的 `props`，`chidren` 是 `props` 中的一个属性，它存储了当前组件的孩子节点，可以是数组（多个孩子节点）或对象（只有一个孩子节点）
- `owner`：当前正在构建的 `Component` 所属的 `Component`
- `self`：（非生产环境）指定当前位于哪个组件实例
- `_source`：（非生产环境）指定调试代码来自的文件(`fileName`)和代码行数(`lineNumber`)

#### 防止 XSS

`ReactElement` 对象还有一个`$$typeof`属性，它是一个`Symbol`类型的变量`Symbol.for('react.element')`，当环境不支持`Symbol`时，`$$typeof` 被赋值为 `0xeac7`。

这个变量可以防止 `XSS`。如果你的服务器有一个漏洞，允许用户存储任意 `JSON` 对象， 而客户端代码需要一个字符串，这可能为你的应用程序带来风险。`JSON` 中不能存储 `Symbol` 类型的变量，而 `React` 渲染时会把没有`$$typeof` 标识的组件过滤掉。

#### 批处理和事务

`React` 在渲染虚拟 `DOM` 时应用了批处理以及事务机制，以提高渲染性能。

#### 针对性的性能优化

在 `IE（8-11）`和`Edge` 浏览器中，一个一个插入无子孙的节点，效率要远高于插入一整个序列化完整的节点树。

`React` 通过 `lazyTree`，在 `IE（8-11）`和 `Edge` 中进行`单个节点依次渲染节点`，而在其他浏览器中则首先将整个大的 DOM 结构构建好，然后再整体插入容器。

并且，在单独渲染节点时，`React` 还考虑了 `fragment` 等特殊节点，这些节点则不会一个一个插入渲染。

#### 虚拟 DOM 事件机制

`React` 自己实现了一套事件机制，其将所有绑定在`虚拟 DOM` 上的事件映射到真正的 `DOM` 事件，并将所有的事件都代理到 `document` 上，自己模拟了事件冒泡和捕获的过程，并且进行统一的事件分发。

`React` 自己构造了合成事件对象 `SyntheticEvent`，这是一个跨浏览器原生事件包装器。 它具有与浏览器原生事件相同的接口，包括`stopPropagation()` 和 `preventDefault()` 等等，在所有浏览器中他们工作方式都相同。这抹平了各个浏览器的事件兼容性问题。
