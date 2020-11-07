---
layout:     post
title:      原生JS快速实现拖放
subtitle:   drag and drop
date:       2020-03-09
author:     huangqing
header-img: img/post-bg-html.jpg
catalog: true
categories: [HTML5]
tags:
    - HTML5
---

## 基本思路

从最古老的方式开始讲起。

`onmousedown`：模拟开始拖拽事件。
+ 鼠标按键按下即发生`onmousedown`事件。
+ 获取鼠标位置，获取被拖拽元素的位置，记录两者之间的纵横坐标的差值。
+ 对`document`元素绑定`onmousemove`,`onmouseup`事件。

为什么是对`document`绑定而不是对被拖动的元素绑定呢？原来是如果对被拖动元素绑定的话当鼠标拖动过快时，会导致鼠标与被拖动元素的脱离。

`onmousemove`：模拟拖拽中事件。
+ 鼠标拖动即发生`onmousemove`事件。
+ 将被拖拽元素的`position`改成绝对位置，这个可以通过`left`和`top`改变该元素的位置，从而使得该元素随着鼠标的拖拽而移动。
+ 获取鼠标位置，将鼠标x坐标（`e.clientX`）减去第2步储存的横坐标差作为被拖动元素的`left`值，将鼠标y坐标（`e.clientY`）减去第2步储存的纵坐标差作为被拖动元素的`top`值。实现元素跟随鼠标拖动的效果。

`onmouseup`：模拟拖拽结束事件。
+ 鼠标按键弹起即发生`onmouseup`事件。
+ 可以回收`onmousemove`和`onmousedown`中的事件和变量，一次拖拽至此结束。

## HTML5拖放

<html>
<head>
<style type="text/css">
#div1 {width:198px; height:66px;padding:10px;border:1px solid #aaaaaa;}
</style>
<script type="text/javascript">
function allowDrop(ev)
{
ev.preventDefault();
}

function drag(ev)
{
ev.dataTransfer.setData("Text",ev.target.id);
}

function drop(ev)
{
ev.preventDefault();
var data=ev.dataTransfer.getData("Text");
ev.target.appendChild(document.getElementById(data));
}
</script>
</head>
<body>

<p>请把 W3School 的图片拖放到矩形中：</p>

<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
<br />
<img id="drag1" src="https://www.w3school.com.cn/i/eg_dragdrop_w3school.gif" draggable="true" ondragstart="drag(event)" />

</body>
</html>

![drag drop](/images/html5/drag-drop.jpg)

#### `draggable`属性

```HTML
<element draggable="true | false | auto" >
```
+ `true`: 可以拖动
+ `false`: 禁止拖动
+ `auto`: 跟随浏览器定义是否可以拖动

#### 7个事件

拖动

每一个可拖动的元素，在拖动过程中，都会经历三个过程，`拖动开始`-->`拖动过程中`--> `拖动结束`。

| 针对对象     | 事件名称    | 说明  |
| -------- | ---------- | ------------- |
| 被拖动的元素 | `dragstart` | 当用户开始拖动对象时触发|
|            | `drag`      | 当对象被拖动，每次移动鼠标时触发  |
|            | `dragend`   | 在拖动对象时放开鼠标按键时触发  |
|            |             |       |
| 目的地对象   | `dragenter` | 当被拖动元素进入目的地元素所占据的屏幕空间时触发，此事件的监听者应指明在这个位置上是否允许`drop` |
|            | `dragover`  | 当被拖动元素在目的地元素内时触发 |
|            | `dragleave` | 当被拖动元素没有放下就离开目的地元素时触发  |

`dragenter`和`dragover`事件的默认行为是拒绝接受任何被拖放的元素。因此，我们必须阻止浏览器这种默认行为。`e.preventDefault()`;

释放

到达目的地之后，释放元素事件

| 针对对象   | 事件名称 | 说明                                                                 |
| ---------- | -------- | -------------------------------------------------------------------- |
| 目的地对象 | `drop`   | 当被拖动元素在目的地元素里放下时触发，一般需要取消浏览器的默认行为。 |

#### `dataTransfer`对象

拖动过程中，回调函数接受的事件参数，有一个`dataTransfer`属性。它指向一个对象，包含了与拖动相关的各种信息。

`dataTransfer`对象的属性:

+ `dropEffect`：拖放的操作类型，决定了浏览器如何显示鼠标形状，可能的值为`copy`、`move`、`link`和`none`。
+ `effectAllowed`：指定所允许的操作，可能的值为`copy`、`move`、`link`、`copyLink`、`copyMove`、`linkMove`、`all`、`none`和`uninitialized`（默认值，等同于all，即允许一切操作）。
+ `files`：包含一个`FileList`对象，表示拖放所涉及的文件，主要用于处理从文件系统拖入浏览器的文件。如果拖动操作不涉及拖动文件，则此属性为空列表。
+ `items`: 提供一个包含所有拖动数据列表的 DataTransferItemList 对象
+ `types`：储存在`DataTransfer`对象的数据的类型。

`dataTransfer`对象的方法：

+ `setData(format, data)`：在`dataTransfer`对象上储存数据。第一个参数`format`用来指定储存的数据类型，比如`text`、`url`、`text/html`等。
+ `getData(format)`：从`dataTransfer`对象取出数据。
+ `clearData(format)`：清除`dataTransfer`对象所储存的数据。如果指定了`format`参数，则只清除该格式的数据，否则清除所有数据。
+ `setDragImage(imgElement, x, y)`：指定拖动过程中显示的图像。默认情况下，许多浏览器显示一个被拖动元素的半透明版本。参数`imgElement`必须是一个图像元素，而不是指向图像的路径，参数x和y表示图像相对于鼠标的位置。

`dataTransfer`对象，允许在其上存储数据，这使得在被拖动元素与目标元素之间传送信息成为可能。

## 示例

HTML

HTML的内容很简单，就是五个空的容器和一个可以被拖拽的元素：

```HTML
<body>
  <div class="droppable">
    <div class="draggable" draggable="true"></div>
  </div>
  <div class="droppable"></div>
  <div class="droppable"></div>
  <div class="droppable"></div>
  <div class="droppable"></div>
</body>
```

注意点：
1. 容器的的`class`为`droppable`，用于接收被拖拽的元素，可被拖拽的元素`class`为`draggable`，同时设置`draggable`属性为`true`，表示该元素可以被拖拽。
2. 默认情况下，只有图片、链接还有被选中的文字能被拖拽，其他元素需要设置`draggable`为`true`才能被拖拽。所以为了凸显`draggable`的用法，这里使用`<div>`而不是`<image>`来作为被拖拽的元素。

CSS

在实现样式的时候，除了实现静态的样式，一些过渡状态也需要增加样式以提升视觉体验：
1. 元素被拖动的过程中增加边框等效果；
2. 当元素被拖动到容器上方时，容器也应增加样式表明容器可以接收一个被拖拽的元素。

```CSS
body {
  background-color: darksalmon;
}

.draggable {
  background-image: url('http://source.unsplash.com/random/150x150');
  position: relative;
  height: 150px;
  width: 150px;
  top: 5px;
  left: 5px;
  cursor: pointer;
}

.droppable {
  display: inline-block;
  height: 160px;
  width: 160px;
  margin: 10px;
  border: 3px salmon solid;
  background-color: white;
}

.dragging {
  border: 4px yellow solid;
}

.drag-over {
  background-color: #f4f4f4;
  border-style: dashed;
}

.invisible {
  display: none;
}
```

注意点：
1. 图片来源于`https://source.unsplash.com/`的随机图片；
2. `.dragging`为`draggable`元素正在被拖动的状态，增加黄色`border`；
3. `.drag-over`为`draggable`元素被拖动到容器上方时容器的状态，增加灰色虚线`border`。

JS

最后，我们需要通过js监听`draggable`和`droppable`的相关的事件。

```JAVASCRIPT
// 查询draggable和droppable
const draggable = document.querySelector('.draggable');
const droppables = document.querySelectorAll('.droppable');

// 监听draggable的相关事件
draggable.addEventListener('dragstart', dragStart);
draggable.addEventListener('dragend', dragEnd);

function dragStart() {
  this.className += ' dragging';
  setTimeout(() => {
    this.className = 'invisible';
  }, 0);
}

function dragEnd() {
  this.className = 'draggable';
}

// 监听droppable的相关事件
for (const droppable of droppables) {
  droppable.addEventListener('dragover', dragOver);
  droppable.addEventListener('dragleave', dragLeave);
  droppable.addEventListener('dragenter', dragEnter);
  droppable.addEventListener('drop', dragDrop);
}

function dragOver(e) {
  e.preventDefault();
}

function dragEnter(e) {
  e.preventDefault();
  this.className += ' drag-over';
}

function dragLeave(e) {
  this.className = 'droppable';
}

function dragDrop(e) {
  this.className = 'droppable';
  this.append(draggable);
}
```

注意点：
1. 当`draggable`元素被拖动时，原来容器中的`draggable`元素并不会消失，需要我们手动将其隐藏（`class`设置为`invisible`），如果同步操作会立马触发`dragend`事件导致拖动效果消失，所以在`setTimeout`的回调中异步设置可确保拖动操作开始后再隐藏`draggable`元素；
2. 在`dragEnter`和`dragOver`方法中我们需要通过`preventDefault`来取消事件以表明容器是一个合法的`droppable`元素，不然容器的`drop`事件将无法触发，接下来的操作也将无法进行，详细解释请参考[MDN DropTarget](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API/Drag_operations#droptargets)；
3. 在`dragDrop`方法中直接使用`append`方法将`draggable`元素移动至当前容器下面。
