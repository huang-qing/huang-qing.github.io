---
layout:     post
title:      Liquid API 语法文档
subtitle:   
date:       2017-10-19
author:     huangqing
header-img: img/post-bg-github-cup.jpg
catalog: true
categories: [GitHub]
tags:
    - jekyll
    - Liquid
---


# Liquid 语法

[liquid 语法网站](https://help.shopify.com/themes/liquid)

{% raw %}

[Liquid](https://github.com/Shopify/liquid/wiki) 是 Ruby 的一个模版引擎库，Jekyll中用到的Liquid标记有两种：输出和标签。

`Output` 标记：变成文本输出，被2层成对的花括号包住，如： `{{content}}` 

`Tag` 标记：执行命令，被成对的花括号和百分号包住，如：  `{% command %}` 



## Jekyll 输出 Output

示例：

```
Hello {{name}}
Hello {{user.name}}
Hello {{ 'tobi' }}
```

`Output` 标记可以使用过滤器 `Filters` 对输出内容作简单处理。
多个 `Filters` 间用`竖线`隔开，从左到右依次执行，`Filter` 左边总是输入，返回值为下一个 `Filter` 的输入或最终结果。

```
Hello {{ 'tobi' | upcase }}  # 转换大写输出
Hello tobi has {{ 'tobi' | size }} letters!  # 字符串长度
Hello {{ '*tobi*' | markdownify | upcase }}  # 将Markdown字符串转成HTML大写文本输出
Hello {{ 'now' | date: "%Y %h" }}  # 按指定日期格式输出当前时间
```

## 标准过滤器 Filters

下面是常用的过滤器方法，更多的API需要查阅源代码（有注释）才能看到。

源码主要看两个 `Ruby Plugin` 文件：`filters.rb(Jekyll)` 和 `standardfilters.rb(Liquid)`。

+ `date` - 将时间戳转化为另一种格式 ([syntax reference](https://help.shopify.com/themes/liquid/filters/additional-filters#date))
+ `capitalize` - 输入字符串首字母大写 e.g. `{{ 'capitalize me' | capitalize }} # => 'Capitalize me'`
+ `downcase` - 输入字符串转换为小写
+ `upcase` - 输入字符串转换为大写
+ `first` - 返回数组中第一个元素
+ `last` - 返回数组数组中最后一个元素
+ `join` - 用特定的字符将数组连接成字符串输出
+ `sort` - 对数组元素排序
+ `map` - 输入数组元素的一个属性作为参数，将每个元素的属性值映射为字符串
+ `size` - 返回数组或字符串的长度 e.g. `{{ array | size }}`
+ `escape` - 将字符串转义输出 e.g.` {{ "<p>test</p>" | escape }} # => <p>test</p>`
+ `escape_once` - 返回转义后的HTML文本，不影响已经转义的HTML实体
+ `strip_html` - 删除 HTML 标签
+ `strip_newlines` - 删除字符串中的换行符(`\n`)
+ `newline_to_br` - 用HTML `<br/>` 替换换行符 `\n`
+ `replace` - 替换字符串中的指定内容 e.g. `{{ 'foofoo' | replace:'foo','bar' }} # => 'barbar'`
+ `replace_first` - 查找并替换字符串中第一处找到的目标子串 e.g. `{{ 'barbar' | replace_first:'bar','foo' }} # => 'foobar'`
+ `remove` - 删除字符串中的指定内容 e.g. `{{ 'foobarfoobar' | remove:'foo' }} # => 'barbar'`
+ `remove_first` - 查找并删除字符串中第一处找到的目标子串 e.g. `{{ 'barbar' | remove_first:'bar' }} # => 'bar'`
+ `truncate` - 截取指定长度的字符串，第2个参数追加到字符串的尾部 e.g. `{{ 'foobarfoobar' | truncate: 5, '.' }} # => 'foob.'`
+ `truncatewords` - 截取指定单词数量的字符串
+ `prepend` - 在字符串前面添加字符串 e.g. `{{ 'bar' | prepend:'foo' }} # => 'foobar'`
+ `append` - 在字符串后面追加字符串 e.g. `{{ 'foo' | append:'bar' }} # => 'foobar'`
+ `slice` - 返回字符子串指定位置开始、指定长度的子串 e.g. `{{ "hello" | slice: -4, 3 }} # => ell`
+ `minus` - 减法运算 e.g. `{{ 4 | minus:2 }} # => 2`
+ `plus` - 加法运算 e.g. `{{ '1' | plus:'1' }} #=> '11', {{ 1 | plus:1 }} # => 2`
+ `times` - 乘法运算 e.g `{{ 5 | times:4 }} # => 20`
+ `divided_by` - 除法运算 e.g. `{{ 10 | divided_by:2 }} # => 5`
+ `split` - 根据匹配的表达式将字符串切成数组 e.g. `{{ "a~b" | split:"~" }} # => ['a','b']`
+ `modulo` - 求模运算 e.g. `{{ 7 | modulo:4 }} # => 3`

## Jekyll 标签 Tag

标签用于模板中的执行语句。目前 `Jekyll/Liquid` 支持的标准标签库有：

|Tags	|说明   |
|----	|----   |
|assign	|为变量赋值|
|capture	|用捕获到的文本为变量赋值|
|case	|条件分支语句 case…when…|
|comment	|注释语句|
|cycle	|通常用于在某些特定值间循环选择，如颜色、DOM类|
|for	|循环语句|
|if	|if/else 语句|
|include	|将另一个模板包进来，模板文件在 `_includes` 目录中|
|raw	|禁用范围内的 Tag 命令，避免语法冲突|
|unless	|if 语句的否定语句|

#### `Comments`

仅起到注释 Liquid 代码的作用。

```javascript
We made 1 million dollars {% comment %} in losses {% endcomment %} this year.
```

#### `Raw`

临时禁止执行 `Jekyll Tag` 命令，在生成的内容里存在冲突的语法片段的情况下很有用。

注意：由于排版问题，去除下面示例中 `{` 与 `%` 之间的空格。

```javascript
{ % raw % }
  In Handlebars, {{ this }} will be HTML-escaped, but {{{ that }}} will not.
{ % endraw % }
```

显示 { % raw % }

```javascript
{% assign openTag = '{%' %}  
{{ openTag }} raw %}    

    content # 代码块   

{{ openTag }} endraw %}
```

#### `If / Else` 

条件语句，可以使用关键字有：`if`、`unless`、`elsif`、`else`。

```javascript
 {% if user %}
   Hello {{ user.name }}
 {% endif %}
 
 # Same as above
 {% if user != null %}
   Hello {{ user.name }}
 {% endif %}
 
 {% if user.name == 'tobi' %}
   Hello tobi
 {% elsif user.name == 'bob' %}
   Hello bob
 {% endif %}
 
 {% if user.name == 'tobi' or user.name == 'bob' %}
   Hello tobi or bob
 {% endif %}
 
 {% if user.name == 'bob' and user.age > 45 %}
   Hello old bob
 {% endif %}
 
 {% if user.name != 'tobi' %}
   Hello non-tobi
 {% endif %}
 
 # Same as above
 {% unless user.name == 'tobi' %}
   Hello non-tobi
 {% endunless %}
 
 # Check for the size of an array
 {% if user.payments == empty %}
    you never paid !
 {% endif %}
 
 {% if user.payments.size > 0  %}
    you paid !
 {% endif %}
 
 {% if user.age > 18 %}
    Login here
 {% else %}
    Sorry, you are too young
 {% endif %}
 
 # array = 1,2,3
 {% if array contains 2 %}
    array includes 2
 {% endif %}
 
 # string = 'hello world'
 {% if string contains 'hello' %}
    string includes 'hello'
 {% endif %}
```

#### `Case` 

适用于当条件实例很多的情况。

```javascript
{% case template %}
{% when 'label' %}
     // {{ label.title }}
{% when 'product' %}
     // {{ product.vendor | link_to_vendor }} / {{ product.title }}
{% else %}
     // {{page_title}}
{% endcase %}
```

#### `Cycle`:

经常需要在相似的任务间选择时，可以使用 `cycle` 标签。

```javascript
{% cycle 'one', 'two', 'three' %}
{% cycle 'one', 'two', 'three' %}
{% cycle 'one', 'two', 'three' %}
{% cycle 'one', 'two', 'three' %}

# =>

one
two
three
one
```

如果要对循环作分组处理，可以指定分组的名字：

```javascript

{% cycle 'group 1': 'one', 'two', 'three' %}
{% cycle 'group 1': 'one', 'two', 'three' %}
{% cycle 'group 2': 'one', 'two', 'three' %}
{% cycle 'group 2': 'one', 'two', 'three' %}

# =>
one
two
one
two
```

#### `For loops`:

循环遍历数组：

```javascript
{% for item in array %}
  {{ item }}
{% endfor %}
```

循环迭代 `Hash` 散列，`item[0]` 是键，`item[1]` 是值：

```javascript
{% for item in hash %}
  {{ item[0] }}: {{ item[1] }}
{% endfor %}
```

每个循环周期，提供下面几个可用的变量：

```javascript
forloop.length      # => length of the entire for loop
forloop.index       # => index of the current iteration
forloop.index0      # => index of the current iteration (zero based)
forloop.rindex      # => how many items are still left ?
forloop.rindex0     # => how many items are still left ? (zero based)
forloop.first       # => is this the first iteration ?
forloop.last        # => is this the last iteration ?
```

还有几个属性用来限定循环过程：

`limit:int` ： 限制循环迭代次数
`offset:int` ： 从第n个item开始迭代
`reversed` ： 反转循环顺序

```javascript
# array = [1,2,3,4,5,6]
{% for item in array limit:2 offset:2 %}
  {{ item }}
{% endfor %}
# results in 3,4

{% for item in collection reversed %}
  {{item}}
{% endfor %}

{% for post in site.posts limit:20 %}
  {{ post.title }}
{% endfor %}
```

允许自定义循环迭代次数，迭代次数可以用常数或者变量说明：

```javascript
# if item.quantity is 4...
{% for i in (1..item.quantity) %}
  {{ i }}
{% endfor %}
# results in 1,2,3,4
```

#### `Variable Assignment` : 

为变量赋值，用于输出或者其他 Tag：

```javascript
{% assign index = 1 %}
{% assign name = 'freestyle' %}

{% for t in collections.tags %}{% if t == name %}
  <p>Freestyle!</p>
{% endif %}{% endfor %}
```

变量是布尔类型

```javascript
{% assign freestyle = false %}

{% for t in collections.tags %}{% if t == 'freestyle' %}
  {% assign freestyle = true %}
{% endif %}{% endfor %}

{% if freestyle %}
  <p>Freestyle!</p>
{% endif %}
```

#### `capture` 

允许将大量字符串合并为单个字符串并赋值给变量，而不会输出显示。

```javascript
{% capture attribute_name %}{{ item.title | handleize }}-{{ i }}-color{% endcapture %}

<label for="{{ attribute_name }}">Color:</label>
<select name="attributes[{{ attribute_name }}]" id="{{ attribute_name }}">
  <option value="red">Red</option>
  <option value="green">Green</option>
  <option value="blue">Blue</option>
</select>
```

## 其他模板语句

#### 字符转义

有时候想输出 `{` 了，怎么办？ 使用反斜线 `\` 转义即可

```javascript
\{ => {
```

#### 格式化时间

```javascript
{{ site.time | date_to_xmlschema }}     # => 2008-11-07T13:07:54-08:00
{{ site.time | date_to_rfc822 }}        # => Mon, 07 Nov 2008 13:07:54 -0800
{{ site.time | date_to_string }}        # => 07 Nov 2008
{{ site.time | date_to_long_string }}   # => 07 November 2008
```

#### 代码语法高亮

安装好 pygments.rb 的 gem 组件和 Python 2.x 后，配置文件添加：highlighter: pygments，就可以使用语法高亮命令了，支持语言多达 100 种以上。

```javascript
{% highlight ruby linenos %}
# some ruby code
{% endhighlight %}
```

上面的示例中，使用 `highlight` 语句来处理代码块；并设定第一个参数 `ruby` 来指定高亮的语言 `Ruby` ，第二个参数 `linenos` 来开启显示代码行号的功能。


#### 链接同域内的 post

使用 `post_url` Tag 可以自动生成网站内的某个 post 超链接。
这个命令语句以相关 post 的文件名为参数，在引入同域的 post 链接时，非常有用。

```javascript
# 自动生成某篇文章的链接地址
{% post_url 2010-07-21-name-of-post %}

# 引入该文章的链接
[Name of Link]({% post_url 2010-07-21-name-of-post %})
```

#### Gist 命令

嵌入 GitHub Gist，也可以指定要显示的 gist 的文件名。

```javascript
{% gist parkr/931c1c8d465a04042403 %}
{% gist parkr/931c1c8d465a04042403 jekyll-private-gist.markdown %}
```

#### 生成摘要

配置文件中设定 `excerpt_separator` 取值，每篇 `post` 都会自动截取从开始到这个值间的内容作为这篇文章的摘要 `post.excerpt` 使用。
如果要禁用某篇文章的摘要，可以在该篇文章的 YAML 头部设定` excerpt_separator: ""` 。

```javascript
{ % for post in site.posts % }
  <a href="{ { post.url } }">{ { post.title } }</a>
  { { post.excerpt | remove: 'test' } }
{ % endfor % }
```

#### 删除 HTML 标签

这个在摘要作为 `head` 标签里的 `meta="description"` 内容输出时很有用

```javascript
{ { post.excerpt | strip_html } }
```

#### 删除指定文本

过滤器 `remove` 可以删除变量中的指定内容

```javascript
{ { post.url | remove: 'http' } }
```

#### CGI Escape

通常用于将 URL 中的特殊字符转义为 `%xx` 形式

```javascript
{ { "foo,bar;baz?" | cgi_escape } }  # => foo%2Cbar%3Bbaz%3F
```

#### 排序

```javascript
# Sort an array. Optional arguments for hashes:
#   1. property name
#   2. nils order ('first' or 'last')

{ { site.pages | sort: 'title', 'last' } }
```

#### 搜索指定 Key

```javascript
# Select all the objects in an array where the key has the given value.
{ { site.members | where:"graduation_year","2014" } } 
```

#### To JSON 格式

将 Hash 散列或数组转换为 JSON 格式

```javascript
{ { site.data.projects | jsonify } }
```

#### 序列化

把一个数组变成一个字符串

```javascript
{ { page.tags | array_to_sentence_string } }  # => foo, bar, and baz
```

#### 单词的个数

```javascript
{ { page.content | number_of_words } }
```

#### 内容名字规范

对于博客 `post` ，文件命名规则必须是 `YEAR-MONTH-DAY-title.MARKUP` 的格式。
使用 `rake post` 会自动将 post 文件合适命名。

比如：

```javascript
2014-11-06-memcached-code.md
2014-11-06-memcached-lib.md
2014-11-06-sphinx-config-and-use.md
2014-11-07-memcached-hash-table.md
2014-11-07-memcached-string-hash.md
```

{% endraw %}