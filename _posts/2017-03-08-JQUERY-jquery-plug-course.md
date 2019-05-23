---
layout:     post
title:      jquery 插件简明教程
subtitle:   
date:       2017-03-08
author:     huangqing
header-img: img/post-bg-jquery.png
catalog: true
categories: [JavaScript]
tags:
    - jquery
---

# jquery 插件简明教程

[basic-plugin-creation](http://learn.jquery.com/plugins/basic-plugin-creation/)

## 基本的插件编写

```javascript
$.fn.greenify=function(){
    this.css("color","green");
}

$("a").greenify();
```

## 链式调用

```javascript
$.fn.greenify=function(){
    this.css("color","green");
    return this;
}

$("a").greenify().addClass("greenified");
```

## 保护`$`操作符并添加作用域

```javascript
(function ( $ ) {
 
    var shade = "#556b2f";
 
    $.fn.greenify = function() {
        this.css( "color", shade );
        return this;
    };
 
}( jQuery ));
```

```javascript
(function ( $ ) {
    //私有变量
    var shade = "#556b2f";
 
    $.fn.greenify = function() {
        this.css( "color", shade );
        return this;
    };
 
}( jQuery ));
```

## 使用 `each()` Method

```javascript
$.fn.myNewPlugin = function() {
 
    return this.each(function() {
        // Do something to each element here.
    });
 
};
```

## 接收参数

```javascript
(function ( $ ) {
 
    $.fn.greenify = function( options ) {
 
        // This is the easiest way to have default options.
        var settings = $.extend({
            // These are the defaults.
            color: "#556b2f",
            backgroundColor: "white"
        }, options );
 
        // Greenify the collection based on the settings variable.
        return this.css({
            color: settings.color,
            backgroundColor: settings.backgroundColor
        });
 
    };
 
}( jQuery ));
```

## 整合一下

```javascript
(function( $ ) {
 
    $.fn.showLinkLocation = function() {
 
        this.filter( "a" ).each(function() {
            var link = $( this );
            link.append( " (" + link.attr( "href" ) + ")" );
        });
 
        return this;
 
    };
 
}( jQuery ));
 
// Usage example:
$( "a" ).showLinkLocation();
```

## 提供默认插件设置公共访问

```javascript
// Plugin definition.
$.fn.hilight = function( options ) {
 
    // Extend our default options with those provided.
    // Note that the first argument to extend is an empty
    // object – this is to keep from overriding our "defaults" object.
    var opts = $.extend( {}, $.fn.hilight.defaults, options );
 
    // Our plugin implementation code goes here.
 
};
 
// Plugin defaults – added as a property on our plugin function.
$.fn.hilight.defaults = {
    foreground: "red",
    background: "yellow"
};

// This needs only be called once and does not
// have to be called from within a "ready" block
$.fn.hilight.defaults.foreground = "blue";
```

## 提供二次函数作为适用的公共访问

```javascript
// Plugin definition.
$.fn.hilight = function( options ) {
 
    // Iterate and reformat each matched element.
    return this.each(function() {
 
        var elem = $( this );
 
        // ...
 
        var markup = elem.html();
 
        // Call our format function.
        markup = $.fn.hilight.format( markup );
 
        elem.html( markup );
 
    });
 
};
 
// Define our format function.
$.fn.hilight.format = function( txt ) {
    return "<strong>" + txt + "</strong>";
};
```
## 保持私有函数

```javascript
// Create closure.
(function( $ ) {
 
    // Plugin definition.
    $.fn.hilight = function( options ) {
        debug( this );
        // ...
    };
 
    // Private function for debugging.
    function debug( obj ) {
        if ( window.console && window.console.log ) {
            window.console.log( "hilight selection count: " + obj.length );
        }
    };
 
    // ...
 
// End of closure.
 
})( jQuery );
```

## 支持回调函数

```javascript
var defaults = {
 
    // We define an empty anonymous function so that
    // we don't need to check its existence before calling it.
    onImageShow : function() {},
 
    // ... rest of settings ...
 
};
 
// Later on in the plugin:
 
nextButton.on( "click", showNextImage );
 
function showNextImage() {
 
    // Returns reference to the next image node
    var image = getNextImage();
 
    // Stuff to show the image here...
 
    // Here's the callback:
    settings.onImageShow.call( image );
}
```

## demo

```javascript
(function ($) {
    // 在插件容器内，创造一个公共变量来构建一个私有方法
    var privateFunction = function () {
        // 执行代码
    }

    // 通过字面量创造一个对象，存储我们需要的共有方法
    var methods = {
        // 在字面量对象中定义每个单独的方法
        init: function (options) {
            // 在每个元素上执行方法：返回“this”（函数each（）的返回值也是this），以便进行链式调用。
            // 为了更好的灵活性，对来自主函数，并进入每个方法中的选择器其中的每个单独的元素都执行代码
            return this.each(function () {

                // 为每个独立的元素创建一个jQuery对象
                var $this = $(this);

                // 尝试去获取settings，如果不存在，则返回“undefined”
                var settings = $this.data('pluginName');

                // 如果获取settings失败，则根据options和default创建它
                if (typeof (settings) == 'undefined') {

                    // 创建一个默认设置对象
                    var defaults = {
                        propertyName: 'value',
                        onSomeEvent: function () {}
                    };

                    // 使用extend方法从options和defaults对象中构造出一个settings对象
                    settings = $.extend({}, defaults, options);

                    // 数据持久化：保存我们新创建的settings
                    $this.data('pluginName', settings);
                } else {
                    // 如果我们获取了settings，则将它和options进行合并（这不是必须的，你可以选择不这样做）
                    settings = $.extend({}, settings, options);

                    // 如果你想每次都保存options，可以添加下面代码：
                    // $this.data('pluginName', settings);
                }

                // 执行代码

            });
        },
        destroy: function (options) {
            // 对选择器每个元素都执行方法
            return $(this).each(function () {
                var $this = $(this);

                // 执行代码

                // 删除元素对应的数据
                $this.removeData('pluginName');
            });
        },
        val: function (options) {
            // 这里的代码通过.eq(0)来获取选择器中的第一个元素的，我们或获取它的HTML内容作为我们的返回值
            var someValue = this.eq(0).html();

            // 返回值
            return someValue;
        }
    };

    //jquery插件：向jQuery中被保护的“fn”命名空间中添加你的插件代码，用“pluginName”作为插件的函数名称
    $.fn.pluginName = function () {
        // 获取我们的方法，遗憾的是，如果我们用function(method){}来实现，这样会毁掉一切的
        var method = arguments[0];

        if (methods[method]) {

            // 如果方法存在，存储起来以便使用
            // 注意：我这样做是为了等下更方便地使用each（）
            method = methods[method];
            // 方法是作为参数传入的，把它从参数列表中删除，因为调用方法时并不需要它
            arguments = Array.prototype.slice.call(arguments, 1);

            // 如果方法不存在，检验对象是否为一个对象（JSON对象）或者method方法没有被传入    
        } else if (typeof (method) == 'object' || !method) {
            // 如果我们传入的是一个对象参数，或者根本没有参数，init方法会被调用
            method = methods.init;

        } else {
            // 如果方法不存在或者参数没传入，则报出错误。需要调用的方法没有被正确调用
            $.error('Method ' + method + ' does not exist on jQuery.pluginName');
            return this;
        }
        // 用apply方法来调用我们的方法并传入参数
        // 调用选中的方法
        // 再一次注意是如何将each（）从这里转移到每个单独的方法上的
        return method.apply(this, arguments);

    }

    // 局部作用域中使用$来引用jQuery
})(jQuery);
```










