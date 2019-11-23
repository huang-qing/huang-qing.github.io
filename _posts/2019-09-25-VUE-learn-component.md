---
layout:     post
title:      Learn Vue - Component
subtitle:   组件
date:       2019-09-25
author:     huangqing
header-img: img/post-bg-vue.png
catalog: true
categories: [Vue]
tags:
    - Vue   
---

## 组件注册

```js
Vue.component('my-component-name', { /* ... */ })
```

强烈推荐遵循 W3C 规范中的自定义组件名 (字母全小写且必须包含一个连字符)。这会帮助你避免和当前以及未来的 HTML 元素相冲突。

#### 全局注册

```js
Vue.component('my-component-name', {
  // ... 选项 ...
})
```

#### 局部注册

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

>注意局部注册的组件在其子组件中不可用

通过 Babel 和 webpack 使用 ES2015 模块，那么代码看起来更像：

```js
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
```

## 模块系统

通过 `import`/`require` 使用一个模块系统

#### 在模块系统中局部注册

`ComponentB.vue` 文件中：

```js
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: {
    ComponentA,
    ComponentC
  },
  // ...
}
```

#### 基础组件的自动化全局注册

如果你使用了 webpack (或在内部使用了 webpack 的 Vue CLI 3+)，那么就可以使用 `require.context` 只全局注册这些非常通用的基础组件。这里有一份可以让你在应用入口文件 (比如 `src/main.js`) 中全局导入基础组件的示例代码：

```js
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // 其组件目录的相对路径
  './components',
  // 是否查询其子目录
  false,
  // 匹配基础组件文件名的正则表达式
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // 获取组件配置
  const componentConfig = requireComponent(fileName)

  // 获取组件的 PascalCase 命名
  const componentName = upperFirst(
    camelCase(
      // 获取和目录深度无关的文件名
      fileName
        .split('/')
        .pop()
        .replace(/\.\w+$/, '')
    )
  )

  // 全局注册组件
  Vue.component(
    componentName,
    // 如果这个组件选项是通过 `export default` 导出的，
    // 那么就会优先使用 `.default`，
    // 否则回退到使用模块的根。
    componentConfig.default || componentConfig
  )
})
```

## Prop

#### Prop类型

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

#### 单向数据流

所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**

这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用

```js
props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
```

这个 prop 以一种原始的值传入且需要进行转换。

```js
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

#### Prop 验证

```js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

类型检查：

1. `String`
2. `Number`
3. `Boolean`
4. `Array`
5. `Object`
6. `Date`
7. `Function`
8. `Symbol`
9. 自定义的构造函数

#### 非 Prop 的特性

一个非 `prop` 特性是指传向一个组件，但是该组件并没有相应 `prop` 定义的特性。

因为显式定义的 `prop` 适用于向一个子组件传入信息，然而组件库的作者并不总能预见组件会被用于怎样的场景。这也是为什么组件可以接受任意的特性，而这些特性会被添加到这个组件的根元素上。

```html
<bootstrap-date-input
  data-date-picker="activated"
  class="date-picker-theme-dark"
></bootstrap-date-input>
```

在这种情况下，我们定义了两个不同的 `class` 的值：

+ `form-control`，这是在组件的模板内设置好的
+ `date-picker-theme-dark`，这是从组件的父级传入的

这个 `data-date-picker="activated"` 特性就会自动添加到 `<bootstrap-date-input>` 的根元素上。`class` 和 `style` 特性会稍微智能一些，即两边的值会被合并起来，从而得到最终的值：`form-control date-picker-theme-dark`。

禁用特性继承

如果你不希望组件的根元素继承特性，你可以在组件的选项中设置 `inheritAttrs: false`。例如：

```js
Vue.component('my-component', {
  inheritAttrs: false,
  // ...
})
```

有了 `inheritAttrs: false` 和 `$attrs`，你就可以手动决定这些特性会被赋予哪个元素。

```js
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on:input="$emit('input', $event.target.value)"
      >
    </label>
  `
})
```
## 自定义事件

```js
this.$emit('myEvent')
```

#### 将原生事件绑定到组件

要在一个组件的根元素上直接监听一个原生事件。这时，你可以使用 `v-on` 的 `.native` 修饰符：

```html
<base-input v-on:focus.native="onFocus"></base-input>
```

Vue 提供了一个 `$listeners` 属性，它是一个对象，里面包含了作用在这个组件上的所有监听器。例如：

有了这个 `$listeners` 属性，你就可以配合 `v-on="$listeners"` 将所有的事件监听器指向这个组件的某个特定的子元素。

```js
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  computed: {
    inputListeners: function () {
      var vm = this
      // `Object.assign` 将所有的对象合并为一个新对象
      return Object.assign({},
        // 我们从父级添加所有的监听器
        this.$listeners,
        // 然后我们添加自定义监听器，
        // 或覆写一些监听器的行为
        {
          // 这里确保组件配合 `v-model` 的工作
          input: function (event) {
            vm.$emit('input', event.target.value)
          }
        }
      )
    }
  },
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on="inputListeners"
      >
    </label>
  `
})
```

#### .sync 修饰符

```html
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```

为了方便起见，我们为这种模式提供一个缩写，即`.sync` 修饰符：

```html
<text-document v-bind:title.sync="doc.title"></text-document>
```

## 插槽

#### 插槽内容

`<navigation-link>`模板:

```html
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

```html
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

当组件渲染的时候，`<slot></slot>` 将会被替换为“Your Profile”:

```html
<navigation-link url="/profile">
  <!-- 添加一个 Font Awesome 图标 -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>
```

甚至其它的组件：

```html
<navigation-link url="/profile">
  <!-- 添加一个图标的组件 -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Your Profile
</navigation-link>
```

#### 后备内容

我们可能希望这个 `<button>` 内绝大多数情况下都渲染文本“Submit”。为了将“Submit”作为后备内容，我们可以将它放在 `<slot>` 标签内：
```html
<button type="submit">
  <slot>Submit</slot>
</button>
```

在当我在一个父级组件中使用 `<submit-button>` 并且不提供任何插槽内容时：
```html
<submit-button></submit-button>
```
后备内容“Submit”将会被渲染：
```html
<button type="submit">
  Submit
</button>
```
但是如果我们提供内容：
```html
<submit-button>
  Save
</submit-button>
```
则这个提供的内容将会被渲染从而取代后备内容：
```html
<button type="submit">
  Save
</button>
```

#### 具名插槽

```html
<div class="container">
  <header>
    <!-- 我们希望把页头放这里 -->
  </header>
  <main>
    <!-- 我们希望把主要内容放这里 -->
  </main>
  <footer>
    <!-- 我们希望把页脚放这里 -->
  </footer>
</div>
```

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

渲染：

```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

#### 作用域插槽

为了让 `user` 在父级的插槽内容中可用，我们可以将 `user` 作为 `<slot>` 元素的一个特性绑定上去：

```html
{% raw %}
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
{% endraw %}
```

```html
{% raw %}
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
{% endraw %}
```

#### 动态插槽名

动态指令参数也可以用在 `v-slot` 上，来定义动态的插槽名：
```html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

#### 具名插槽的缩写

跟 `v-on` 和 `v-bind` 一样，`v-slot` 也有缩写，即把参数之前的所有内容 (`v-slot:`) 替换为字符 `#`。例如 `v-slot:header` 可以被重写为 `#header`。

如果你希望使用缩写的话，你必须始终以明确插槽名取而代之：
```html
{% raw %}
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
{% endraw %}
```

推荐浏览诸如 
+ [Vue Virtual Scroller](https://github.com/Akryum/vue-virtual-scroller)
+ [Vue Promised](https://github.com/posva/vue-promised)
+ [Portal Vue](https://github.com/LinusBorg/portal-vue)

## 动态组件 & 异步组件

#### 在动态组件上使用 `keep-alive`

多标签的界面中使用 `is` 特性来切换不同的组件：

```html
<component v-bind:is="currentTabComponent"></component>
```

```html
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

#### 异步组件

推荐的做法是将异步组件和 [webpack 的 code-splitting 功能](https://webpack.js.org/guides/code-splitting/)一起配合使用：

```js
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 `require` 语法将会告诉 webpack
  // 自动将你的构建代码切割成多个包，这些包
  // 会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
```

也可以在工厂函数中返回一个 `Promise`，所以把 webpack 2 和 ES2015 语法加在一起，我们可以写成这样：

```js
Vue.component(
  'async-webpack-example',
  // 这个 `import` 函数会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
```

当使用局部注册的时候，你也可以直接提供一个返回 `Promise` 的函数：

```js
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```

#### 处理加载状态

这里的异步组件工厂函数也可以返回一个如下格式的对象：

```js
const AsyncComponent = () => ({
  // 需要加载的组件 (应该是一个 `Promise` 对象)
  component: import('./MyComponent.vue'),
  // 异步组件加载时使用的组件
  loading: LoadingComponent,
  // 加载失败时使用的组件
  error: ErrorComponent,
  // 展示加载时组件的延时时间。默认值是 200 (毫秒)
  delay: 200,
  // 如果提供了超时时间且组件加载也超时了，
  // 则使用加载失败时使用的组件。默认值是：`Infinity`
  timeout: 3000
})
```

#### 处理边界情况

访问根实例:`this.$root`

访问父级组件实例:`$parent` 属性可以用来从一个子组件访问父组件的实例

访问子组件实例或子元素:`<base-input ref="usernameInput"></base-input>`,`this.$refs.usernameInput`

#### 依赖注入

在此之前，在我们描述访问父级组件实例的时候，展示过一个类似这样的例子：

```html
<google-map>
  <google-map-region v-bind:shape="cityBoundaries">
    <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
  </google-map-region>
</google-map>
```

在这个组件里，所有 `<google-map>` 的后代都需要访问一个 `getMap` 方法，以便知道要跟哪个地图进行交互。不幸的是，使用 `$parent` 属性无法很好的扩展到更深层级的嵌套组件上。这也是依赖注入的用武之地，它用到了两个新的实例选项： `provide` 和 `inject` 。

`provide` 选项允许我们指定我们想要提供给后代组件的数据/方法。在这个例子中，就是 `<google-map>` 内部的 `getMap` 方法：

```js
provide: function () {
  return {
    getMap: this.getMap
  }
}
```

然后在任何后代组件里，我们都可以使用 `inject` 选项来接收指定的我们想要添加在这个实例上的属性：

```js
inject: ['getMap']
```

相比 `$parent` 来说，这个用法可以让我们在任意后代组件中访问 `getMap`，而不需要暴露整个 `<google-map>` 实例。这允许我们更好的持续研发该组件，而不需要担心我们可能会改变/移除一些子组件依赖的东西。同时这些组件之间的接口是始终明确定义的，就和 `props` 一样。

实际上，你可以把依赖注入看作一部分“大范围有效的 prop”，除了：

+ 祖先组件不需要知道哪些后代组件使用它提供的属性
+ 后代组件不需要知道被注入的属性来自哪里

#### 程序化的事件侦听器

+ 通过 `$on(eventName, eventHandler)` 侦听一个事件
+ 通过 `$once(eventName, eventHandler)` 一次性侦听一个事件
+ 通过 `$off(eventName, eventHandler)` 停止侦听一个事件

```js
mounted: function () {
  var picker = new Pikaday({
    field: this.$refs.input,
    format: 'YYYY-MM-DD'
  })

  this.$once('hook:beforeDestroy', function () {
    picker.destroy()
  })
}
```