---
layout:     post
title:      Jquery多库共存的方案
subtitle:   
date:       2020-03-09
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [JavaScript]
tags:
    - JavaScript
---

`Jquery`有多库共存的方案，那就是`Jquery.noconfilct()`功能。

```html
<script type="text/javascript" src="jquery-1.9.1.js"></script>
<script type="text/javascript" src="customize.js"></script>
<script type="text/javascript">
var _$ = jQuery.noConflict(true);
</script>
<script type="text/javascript" src="jquery-1.6.2.js"></script>
<script type="text/javascript">
//invoke Jquery.1.6.2
$("document").ready(function(){
  alert("faf");
})
//invoke Jquery.1.9.1
_$("document").ready(function(){
  alert("faf");
})
</script>
```

这里只是简单的整合两个版本，`_$`调用的是`1.9.1`版本的API,而`$`调用的是`1.6.3`的API.

通常我们都会基于某个版本写插件，比如基于1.9.1的版本写了个`customize.js`,这个文件必须放在`1.9.1.js`引用之后， `jQuery.noConflict(true)`之前。这里需要注意，`customize.js`里面的插件编写，还是必须用"`$`"，这是因为此时还没有调用`jQuery.noConflict`函数，在调用完该函数后，需要用到自定义的插件或是1.9.1里的函数库，就必须用"`_$`"。如`_$("#selector").company_button`调用的就是自定义的插件。如调用`$("#selector").company_button`，就会出现无此函数的错误。