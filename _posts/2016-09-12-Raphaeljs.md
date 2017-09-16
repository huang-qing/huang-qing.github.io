---
layout:     post
title:      Raphael（拉斐尔）
subtitle:   
date:       2016-09-12
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [javascript]
tags:
    - Raphael
---

# Raphael（拉斐尔）

+ [raphael](http://dmitrybaranovskiy.github.io/raphael/)
+ [raphael reference](http://dmitrybaranovskiy.github.io/raphael/reference.html)
+ [raphael github](https://github.com/DmitryBaranovskiy/raphael)

## 参考

+ [timeline](https://github.com/huang-qing/timeline)
+ [topology-diagram](https://github.com/huang-qing/topology-diagram)


## paper

~~~javascript

var paper = Raphael(0, 0, 600, 600);

paper.clear();

paper.setSize(width, height);

paper.setViewBox(viewBoxX, viewBoxY, width, height, false);
~~~


## 画矩形rect

~~~javascript

TopologyDiagram.prototype.createRect = function (x, y, width, id) {
    var config = this.config.rect,
        paper = this.paper.element,
        rect;

    rect = paper.rect(x, y, width, config.height, config.radius).attr({
        fill: config.fill['default'],
        'stroke-width': config['stroke-width'],
        stroke: config.stroke['default'],
        r: config.radius
    });

    $(rect[0]).attr({
        'data-nodeId': id,
        'id': id + '-rect'
    }).addClass('node node-rect');

    return rect;
};

~~~

## 图片image

~~~javascript

TopologyDiagram.prototype.createImage = function (x, y, src, id, index) {
    var config = this.config.image,
        paper = this.paper.element,
        image;

    image = paper.image(src, x, y, config.width, config.height).attr({

    });

    $(image[0]).attr({
        'data-nodeId': id,
        'id': id + '-' + index + '-image'
    }).addClass('node node-image');

    return image;
};
~~~

## 文字text

~~~javascript

TopologyDiagram.prototype.createText = function (x, y, text, id) {
    var config = this.config.text,
        maxLength = config.maxLength,
        paper = this.paper.element,
        title = text,
        textElem;

    if (text.length > maxLength) {
        text = text.slice(0, maxLength) + '...';
    }
    // 文字是以开始位置文字的水平中轴线为基线
    y = y + (config['font-size'] / 2) + 1;
    textElem = paper.text(x, y, text).attr({
        'font-size': config['font-size'],
        'text-anchor': 'start',
        'title': title,
        fill: config.fill['default'],
        id: id + '-text'
    });

    $(textElem[0]).attr({
        'data-nodeId': id,
        'id': id + '-text'
    }).addClass('node node-text');

    return textElem;
};
    
~~~

## 置前

~~~javascript
element.toFront();
~~~

## css 文字选中效果

~~~css
.topology-diagram tspan::selection {
    background: #f4f4f4;
}
~~~


## 画箭头 

`Element.attr(…)`方法的`arrow-end`参数：

~~~
arrow-end string  
arrowhead on the end of the path. The format for string is <type>[-<width>[-<length>]].   
Possible types: classic, block, open, oval, diamond, none, width: wide, narrow, midium, length: long, short, midium.
~~~


~~~javascript
$(function () {

    // 在坐标（10,50）创建宽320，高200的画布  
    var paper = Raphael(0, 0, 3200, 2000);

    var line1 = paper.path("M100,200 L 200,400").attr({
        stroke: "red",
        "stroke-width": "2px",
        "arrow-end": "classic-wide-long"
    });

    var line2 = paper.path("M500,200 L 600,400").attr({
        stroke: "green",
        "stroke-width": "2px",
        "arrow-end": "classic-wide-long"

    });
    //
    if (Raphael.svg) {
        line1.node.attributes["marker-end"].value = "url(#raphael-marker-endclassic-" + "red" + ")";
        line2.node.attributes["marker-end"].value = "url(#raphael-marker-endclassic-" + "green" + ")";
    }

});
~~~

## 箭头兼容性问题

上面的代码创建了两个带箭头的path。在IE浏览器下，没有任何问题；在谷歌浏览器下，绿色的箭头是迟于红色箭头实例的，箭头会被后者的颜色所覆盖。

Raphael为箭头的`marker-end`属性设置了一个引用地址`url(#raphael-marker-endclassic55)`，这个是`classic-wide-long`属性自己生成的，而这个`raphael-marker-endclassic55`就存在于svg画布中。

解决方案：
+ Raphael 2.2.0 已修复这个问题
+ 之前的版本调整 addArrow 方法




