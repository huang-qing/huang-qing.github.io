---
layout:     post
title:      Learn Vue - Base
subtitle:   基础
date:       2019-09-24
author:     huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vue]
tags:
    - Vue   
---


## 安装

调试工具：Vue Devtools

脚本引入：
+ 直接使用`<script>`:开发版、生产版
+ CDN: jsdelivr \ unpkg \ cdnjs
+ NPM: `npm install vue`

命令行工具（CLI）：
+ `SPA`单页面应用
+ `batteries-included`构建设置
+ 热重载
+ 保存时`lint`校验
+ 生产环境构建版本

构建版本类型：
1. UMD：通过 `<script>` 标签直接用在浏览器中
2. CommonJS: 配合老的打包工具比如 Browserify 或 webpack 1
3. ES Module（基于构建工具）
   + 诸如 `webpack 2` 或 `Rollup` 提供的现代打包工具。
   + ESM 格式被设计为可以被静态分析,打包工具可以利用这一点来进行“`tree-shaking`”并将用不到的代码排除出最终的包。
4. ES Module（基于浏览器）: 用于在现代浏览器中通过 `<script type="module">` 直接导入。

运行时 + 编译器 vs. 只包含运行时:

为运行时版本相比完整版体积要小大约 30%，所以生产环境应该尽可能使用这个版本

开发环境 vs. 生产环境模式：

+ 保留原始的 `process.env.NODE_ENV` 检测，以决定它们应该运行在什么模式下。
+ `UglifyJS` 压缩

CSP环境：有些环境，如 Google Chrome Apps，会强制应用内容安全策略 (CSP)，不能使用 `new Function()` 对表达式求值。这时可以用 CSP 兼容版本。

## 介绍

声明式渲染：

```HTML
<div id="app">
  {{ message }}
</div>    
```

```JAVASCRIPT
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

指令：`v-`

条件与循环： `v-if` `v-for`

```html
<p v-if="seen">现在你看到我了</p>
```

```html
<li v-for="todo in todos">
    {{ todo.text }}
</li>
```

处理用户输入：`v-on:click`

```html
 <button v-on:click="reverseMessage">反转消息</button>
```

```javascript
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
```

双向绑定：`v-model`

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

组件化应用构建: 在Vue实例创建前声明

```javascript
Vue.component('todo-item', {
  // todo-item 组件现在接受一个
  // "prop"，类似于一个自定义特性。
  // 这个 prop 名为 todo。
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

```html
<div id="app-7">
  <ol>
    <!--
      现在我们为每个 todo-item 提供 todo 对象
      todo 对象是变量，即其内容可以是动态的。
      我们也需要为每个组件提供一个“key”，稍后再
      作详细解释。
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>

```

## Vue实例

实例生命周期钩子：

![](/images/vue/lifecycle.png)

## 模板语法

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：

```html
<span>Message: {{ msg }}</span>
```

`v-once`指令：执行一次性地插值
```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

`v-html`:原始 HTML
```html
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

`v-bind`:

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

对于布尔特性,如果 `isButtonDisabled` 的值是 `null`、`undefined` 或 `false`，则 `disabled` 特性甚至不会被包含在渲染出来的 `<button>` 元素中。

动态参数：

```html
<!--
注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。
-->
<a v-bind:[attributeName]="url"> ... </a>
```

这里的 `attributeName` 会被作为一个 `JavaScript` 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 `Vue` 实例有一个 `data` 属性 `attributeName，其值为` "`href`"，那么这个绑定将等价于 `v-bind:href`。

同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：
```html
<a v-on:[eventName]="doSomething"> ... </a>
```

修饰符：修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

缩写:

`v-bind` -> `:`

`v-on` -> `@`


## 计算属性和侦听器

对于任何复杂逻辑，你都应当使用计算属性:

```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：
```javascript
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

侦听器:`watch`

当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。
```html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

```html
<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
```

## Class 与 Style 绑定

### 绑定 HTML Class

#### 对象语法

我们可以传给 `v-bind:class` 一个对象，以动态地切换 class：

```html
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```

```js
data: {
  isActive: true,
  hasError: false
}
```

```html
<div class="static active"></div>
```

上面的语法表示 `active` 这个 class 存在与否将取决于数据属性 `isActive` 的 truthiness。

绑定的数据对象不必内联定义在模板里：

```html
<div v-bind:class="classObject"></div>
```

```js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

#### 数组语法

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```
```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
渲染为：
```html
<div class="active text-danger"></div>
```

### 绑定内联样式

`v-bind:style`

#### 对象语法

```html
<div v-bind:style="styleObject"></div>
```

```js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

#### 数组语法

数组语法可以将多个样式对象应用到同一个元素上：

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

#### 多重值

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 `flexbox`，那么就只会渲染 `display: flex`。


## 条件渲染

#### 条件`v-if` `v-else` `v-else-if`

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

#### 用 key 管理可复用的元素

 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key 属性即可：

 ```html
 <template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
 ```

 #### `v-if` vs `v-show`

 `v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

## 列表渲染

### 用`v-for`把一个数组对应为一组元素

我们可以用 `v-for` 指令基于一个数组来渲染一个列表。`v-for` 指令需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据数组，而 `item` 则是被迭代的数组元素的别名。

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

<ul>
  <li>Parent - 0 - Foo</li>
  <li>Parent - 1 - Bar</li>
</ul>

可以用 `of` 替代 `in` 作为分隔符，因为它更接近 JavaScript 迭代器的语法：

```html
<div v-for="item of items"></div>
```

### 在 `v-for` 里使用对象

```html
{% raw %}
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
{% endraw %}
```

### 维护状态

当 Vue 正在更新使用 v-for 渲染的元素列表时，它默认使用“就地更新”的策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。

这个默认的模式是高效的，但是**只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。

建议尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升

### 数组更新检测

#### 变异方法（mutation method）

+ `push()`
+ `pop()`
+ `shift()`
+ `unshift()`
+ `splice()`
+ `sort()`
+ `reverse()`

#### 替换数组

变异方法，顾名思义，会改变调用了这些方法的原始数组。相比之下，也有非变异 (non-mutating method) 方法，例如 filter()、concat() 和 slice() 。它们不会改变原始数组，而**总是返回一个新数组**。

```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

#### 注意事项

由于 JavaScript 的限制，Vue 不能检测以下数组的变动：

1. 当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`
2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

使用以下方式：

```js
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)

vm.$set(vm.items, indexOfItem, newValue)
```

```js
// Array.prototype.splice
// 直接对数组进行修改
vm.items.splice(indexOfItem, 1, newValue)

vm.items.splice(newLength)
```

### 显示过滤/排序后的结果

```html
{% raw %}
<li v-for="n in evenNumbers">{{ n }}</li>
{% endraw %}
```

```js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个方法：
```html
{% raw %}
<li v-for="n in even(numbers)">{{ n }}</li>
{% endraw %}
```

```js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

### 在 v-for 里使用值范围

```html
{% raw %}
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
{% endraw %}
```
```
1 2 3 4 5 6 7 8 9 10
```

### 在 `template` 上使用 `v-for`

```html
{% raw %}
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
{% endraw %}
```

### 在组件上使用 v-for

```html
{% raw %}
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
{% endraw %}
```

## 事件处理

#### 事件处理方法

```html
<div id="example-2">
  <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>
```

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

```js
// ...
methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) event.preventDefault()
    alert(message)
  }
}
```

#### 事件修饰符

+ `.stop`     阻止事件继续传播
+ `.prevent`  阻止默认事件
+ `.capture`  使用事件捕获模式，即内部元素触发的事件先在此处理，然后才交由内部元素进行处理
+ `.self`     只当在 event.target 是当前元素自身时触发处理函数
+ `once`      事件将只会触发一次
+ `passive`   滚动事件的默认行为 (即滚动行为) 将会立即触发，而不会等待 `onScroll` 完成

#### 按键修饰符

+ `.enter`
+ `.tab`
+ `.delete` (捕获“删除”和“退格”键)
+ `.esc`
+ `.space`
+ `.up`
+ `.down`
+ `.left`
+ `.right`

#### 系统修饰键

+ `.ctrl`
+ `.alt`
+ `.shift`
+ `.meta`

#### `.exact` 修饰符

```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

#### 鼠标按钮修饰符

+ `.left`
+ `.right`
+ `.middle`

## 表单输入绑定

v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

+ `text` 和 `textarea` 元素使用 `value` 属性和 `input` 事件；
+ `checkbox` 和 `radio` 使用 `checked` 属性和 `change` 事件；
+ `select` 字段将 `value` 作为 `prop` 并将 `change` 作为事件。

```html
<input v-model="message" placeholder="edit me">
```

```html
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```

#### 修饰符

+ `.lazy`
+ `.number`
+ `.trim`


## 组件基础

```js
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  //data 必须是一个函数
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```js
new Vue({ el: '#components-demo' })
```

这里有两种组件的注册类型：全局注册和局部注册。
```js
// 全局注册
Vue.component('my-component-name', {
  // ... options ...
})
```

#### 通过 Prop 向子组件传递数据

```js
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `
})
```
```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post"
></blog-post>
```

#### 监听子组件事件

```js
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button v-on:click="$emit('enlarge-text',0.1)">
        Enlarge text
      </button>
      <div v-html="post.content"></div>
    </div>
  `
})
```

```html
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```

```js
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

#### 在组件上使用 `v-model`

```html
<input v-model="searchText">
```
等价于：

```html
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```
当用在组件上时，v-model 则会这样：

```html
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```

为了让它正常工作，这个组件内的 `<input>` 必须：

+ 将其 value 特性绑定到一个名叫 value 的 prop 上
+ 在其 input 事件被触发时，将新的值通过自定义的 input 事件抛出

```js
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```

```html
<custom-input v-model="searchText"></custom-input>
```

#### 通过插槽分发内容

```html
<alert-box>
  Something bad happened.
</alert-box>
```

```
Error!Something bad happended.
```

```js
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```

#### 动态组件

上述内容可以通过 Vue 的 `<component>` 元素加一个特殊的 `is` 特性来实现：

```html
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component>
```
在上述示例中，`currentTabComponent` 可以包括

+ 已注册组件的名字
+ 一个组件的选项对象