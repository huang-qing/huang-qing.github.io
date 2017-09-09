---
layout:     post
title:      Markdown 语法说明
subtitle:   总结Markdown的常用语法
date:       2017-01-02
author:     huangqing
header-img: img/post-bg-markdown.jpg
catalog: true
categories: [Markdown]
tags:
    - Markdown
---

# Markdown语法说明

![markdown-logo.png](/images/markdown/947566-5c4a6d0c90dee65d.png)

Markdown 的目标是实现「易读易写」。 

## 行内HTML

+  不在 Markdown 涵盖范围之内的标签，可以直接在文档里面用 HTML 撰写。

+  例外：块区元素`<div> <table> <pre> <p>`，必须在前后加上空白。且不可用空白来缩排。

## 特殊字符自动转换

例如：在 HTML 文档中，有两个字符需要特殊处理： `<` 和 `&`。

## 标题：

Markdown 支持两种标题的语法，[Setext][]和 [atx][] 形式。 

[Setext]: http://docutils.sourceforge.net/mirror/setext.html
[atx]: http://www.aaronsw.com/2002/atx/

+  Setext 形式是用底线的形式，利用` =`（一级标题）和` -`（二级标题）

+  Atx 形式则是在行首插入 1 到 6 个 `#`，对应到标题 1 到 6 

```
# 一级标题

## 二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题
```


## 引言:Blockquotes

+  Markdown 文档中建立一个区块引言，那会看起来像是强迫断行，然后在每行的最前面加上` >`

+  允许只在整个段落的第一行最前面加上 `>`

+  区块引言可以有级别（例如：引言内的引言），只要根据级别加上不同数量的 `>`

+  引言的区块内也可以使用其他的 Markdown 语法，包括标题、列表、程序代码区块等

## 列表

Markdown 支持有序列表和无序列表

+  无序列表使用星号`*`、加号`+`或是减号`-`作为列表标记

+  有序列表则使用数字接着一个英文句点

+  列表项目可以包含多个段落，每个项目下的段落都必须缩排 4 个空白或是一个 `tab`

## 程序代码区块

Markdown 会用 `<pre>`和`<code>`标签来把程序代码区块包起来。

+  代码块：缩排 4个空白或是 1 个` tab` 就可以；或直接使用`<pre></pre>`

+  高亮：使用 <code>\`</code> 或直接使用 `<code></code>` 

+  使用<code>```或~~~</code>包裹一段代码，并指定一种语言：如:javascript,json,html,css

<pre>
```javascript
$(document).ready(function () {
    alert('hello world');
});
```
</pre>

支持的语言：`actionscript, apache, bash, clojure, cmake, coffeescript, cpp, cs, css, d, delphi, django, erlang, go, haskell, html, http, ini, java, javascript, json, lisp, lua, markdown, matlab, nginx, objectivec, perl, php, python, r, ruby, scala, smalltalk, sql, tex, vbscript, xml`


## 分隔线

在一行中用三个或以上的星号`*`、减号`-`、底线`_`来建立一个分隔线，行内不能有其他东西

***

## 链接

Markdown 支持两种形式的链接语法： 「行内」和「参考」两种形式。

+  建立一个行内形式的链接，只要在方块括号后面马上接着括号并插入网址链接即可。

+  参考形式的链接使用另外一个方括号接在链接文字的括号后面，而在第二个方括号里面要填入用以辨识链接的标签；接着，在文档的任意处，可以把这个标签的链接内容定义出来。

## 强调

Markdown 使用星号`*`和底线`_`作为标记强调字词的符号。

+  被 `*`或` _`包围的字词会被转成用 `<em>`标签包围。

+  用两个 `*`或` _`包起来的话，则会被转成` <strong>`。

+  如果`*`和` _`两边都有空白的话，它们就只会被当成普通的符号。

+  如果要在文字前后直接插入普通的星号或底线，可以用反斜杠


## 图片

Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式： 「行内」和「参考」。

+  行内语法：

~~~markdown
![Alt text](/path/to/img.jpg )
![Alt text](/path/to/img.jpg "Optional title")
~~~

+  参考语法：

~~~markdown

![Alt text][id]

~~~

`「id」`是图片参考的名称，图片参考的定义方式则和链接参考一样：<pre>[id]:url/to/image "Optional title attribute"</pre>
 
## 自动链接

Markdown 支持比较简短的自动链接形式来处理网址和电子邮件信箱，例如：

~~~html
<http://example.com/>

<a href="http://example.com/">http://example.com/</a>
~~~

## 转义字符

Markdown 可以利用<code>\</code>反斜杠来插入一些在语法中有其它意义的符号。

支持在下面这些符号前面加上反斜杠来帮助插入普通的符号：

<pre>
\    反斜杠
`    反引号
\*    星号
_    底线
{}   大括号
[]   方括号
()   括号
\#    井字号
\+    加号
\-    减号
.    英文句点
!    惊叹号
</pre>


## 表格

~~~

|方法	|安全	|幂等	|
|----	|----	|----	|
|GET	|YES	|YES	|
|HEAD	|YES	|YES	|
|OPTIONS|YES	|YES	|
|PUT	|NO     |YES	|
|DELETE	|NO     |YES	|
|POST	|NO     |NO |

~~~

|方法	|安全	|幂等	|
|----	|----	|----	|
|GET	|YES	|YES	|
|HEAD	|YES	|YES	|
|OPTIONS|YES    |YES	|
|PUT	|NO		|YES	|
|DELETE	|NO		|YES	|
|POST	|NO		|NO		|


## 参考文档

+  [Markdown语法说明（详解版）](http://www.ituring.com.cn/article/504)

+  [Markdown官方文档](http://markdown.tw/)

+  [作业部落](https://www.zybuluo.com/mdeditor)