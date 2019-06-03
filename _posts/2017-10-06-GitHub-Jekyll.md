---
layout:     post
title:      Jekyll
subtitle:   将纯文本转换为静态博客网站
date:       2017-10-06
author:     huangqing
header-img: img/post-bg-github-cup.jpg
catalog: true
categories: [GitHub]
tags:
    - jekyll
---

## 中文网站

[jekyllcn](http://jekyllcn.com/)

## 快速开始

```javascript

~ $ gem install jekyll bundler
~ $ jekyll new my-awesome-site
~ $ cd my-awesome-site
~/my-awesome-site $ bundle install
~/my-awesome-site $ bundle exec jekyll serve
# => 打开浏览器 http://localhost:4000

```
使用 GitHub 的免费主机

[GitHub Pages](https://pages.github.com/) 基于 Jekyll 构建，你可以轻而易举地在 GitHub 上免费发布网站——[自定义域名](https://help.github.com/articles/about-supported-custom-domains/)等等。

## 目录结构

`Jekyll` 的核心其实是一个文本转换引擎。它的概念其实就是：你用你最喜欢的标记语言来写文章，可以是 `Markdown`, 也可以是 `Textile`, 或者就是简单的 `HTML`, 然后 `Jekyll` 就会帮你套入一个或一系列的布局中。
在整个过程中你可以设置 `URL` 路径，你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，最终生成的静态页面就是你的成品了。

#### 基本的目录结构:

一个基本的 Jekyll 网站的目录结构一般是像这样的：

```
.
├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _site
├── .jekyll-metadata
└── index.html

```

#### 文件/目录 描述:

` _config.yml`:

保存配置数据。很多配置选项都可以直接在命令行中进行设置，但是如果你把那些配置写在这儿，你就不用非要去记住那些命令了。

` _drafts`:

drafts（草稿）是未发布的文章。这些文件的格式中都没有 `title.MARKUP` 数据。命令最后添加 `--drafts` 配置选项。

` _includes`:

你可以加载这些包含部分到你的布局或者文章中以方便重用。

` _layouts`:

layouts（布局）是包裹在文章外部的模板。布局可以在 `YAML` 头信息中根据不同文章进行选择。

` _posts`:

这里放的就是你的文章了。文件格式很重要，必须要符合: `YEAR-MONTH-DAY-title.MARKUP`。 `永久链接`可以在文章中自己定制，但是数据和标记语言都是根据文件名来确定的。

`_data`:

格式化好的网站数据应放在这里。`jekyll` 的引擎会自动加载在该目录下所有的 `yaml` 文件（后缀是 `.yml`, `.yaml`, `.json` 或者 `.csv` ）。这些文件可以经由 ｀site.data｀ 访问。如果有一个 `members.yml` 文件在该目录下，你就可以通过 `site.data.members` 获取该文件的内容。

`_site`:

一旦 `Jekyll` 完成转换，就会将生成的页面放在这里（默认）。最好将这个目录放进你的 `.gitignore` 文件中。

`.jekyll-metadata`:

该文件帮助 Jekyll 跟踪哪些文件从上次建立站点开始到现在没有被修改，哪些文件需要在下一次站点建立时重新生成。该文件不会被包含在生成的站点中。将它加入到你的 .gitignore 文件可能是一个好注意。

`index.html` and `other HTML, Markdown, Textile files`:

如果这些文件中包含 `YAML` 头信息 部分，`Jekyll` 就会自动将它们进行转换。当然，其他的如 `.html`, `.markdown`, `.md`, 或者 `.textile` 等在你的站点根目录下或者不是以上提到的目录中的文件也会被转换。

`Other Files/Folders`:

其他一些未被提及的目录和文件如  `css` 还有 `images` 文件夹，`favicon.ico` 等文件都将被完全拷贝到生成的 `site` 中。



## 配置

Jekyll允许你很轻松的设计你的网站，这很大程度上归功于灵活强大的配置功能。既可以配置在网站根目录下的  `_config.yml` 文件，也可以作为命令行的标记来配置。[详细参见](http://jekyllcn.com/docs/configuration/)

## 头信息

正是头信息开始让 `Jekyll` 变的很酷。任何只要包含 `YAML` 头信息的文件在 `Jekyll` 中都能被当做一个特殊的文件来处理。头信息必须在文件的开始部分，并且需要按照 `YAML` 的格式写在两行三虚线之间。下面是一个基本的例子：

```javascript
---
layout:     post   # 指定使用的模板文件，“_layout” 目录下的模板文件名决定变量名
title:      title  # 文章的标题
date:       date   # 覆盖文章名中的日期
category:   blog   # 文章的类别
description: description
published:  true   # default true 设置 “false” 后，文章不会显示
permalink:  /:categories/:year/:month/:day/:title.html  # 覆盖全局变量设定的文章发布格式
---
```

注意：如果文本文件使用的是 `utf-8` 编码，那么必须确保文件中不存在 BOM 头部字符，尤其是当 `Jekyll` 运行在 `Windows` 平台上。

在这两行的三虚线之间，你可以设置预定义的变量，甚至创建一个你自己定义的变量。这样在接下来的文件和任意模板中或者在包含这些页面或博客的模板中都可以通过使用 `Liquid` 标签来访问这些变量。

### 预定义的全局变量

你可以在页面或者博客的头信息里设置这些已经预定义好的全局变量。

`layout`:

如果设置的话，会指定使用该模板文件。指定模板文件时候不需要文件扩展名。模板文件必须放在 `_layouts` 目录下。

`permalink`:

如果你需要让你发布的博客的 `URL` 地址不同于默认值 `/year/month/day/title.html`，那你就设置这个变量，然后变量值就会作为最终的 `URL` 地址。

`published`:

如果你不想在站点生成后展示某篇特定的博文，那么就设置（该博文的）该变量为 false。

### 文章中预定义的变量

在文章中可以使用这些在头信息变量列表中未包含的变量。

`date`:

这里的日期会覆盖文章名字中的日期。这样就可以用来保障文章排序的正确。日期的具体格式为`YYYY-MM-DD HH:MM:SS +/-TTTT`；时，分，秒和时区都是可选的。

`category` `categories`:

除过将博客文章放在某个文件夹下面外，你还可以指定博客的一个或者多个分类属性。这样当你的站点生成后，这些文章就可以根据这些分类来阅读。`categories` 可以通过 `YAML list`，或者以`逗号`隔开的字符串指定。

`tags`:

类似分类 `categories`，一篇文章也可以给它增加一个或者多个标签。同样，`tags` 可以通过 `YAML` 列表或者以`逗号`隔开的字符串指定。

### 自定义变量

在头信息中没有预先定义的任何变量都会在数据转换中通过 Liquid 模板被调用。
例如，如果你设置一个 `title` 变量，然后你就可以在你的模板中使用这个 `title` 变量来设置页面的标题（title）：

```html
<!DOCTYPE HTML>
<html>
  <head>
    <title>{{ page.title }}</title>
  </head>
  <body>
    ...
```

## 静态文件

除了用于渲染和转换的内容之外，我们还可以使用静态文件。静态文件不包含任何 YAML 头信息，譬如图片、PDF 和其他不必渲染的内容。
它们在 Liquid 中可以通过 `site.static_files` 访问


#### 元数据

`file.path`:

文件的相对路径，如：`/assets/img/image.jpg`

`file.modified_time`:

文件的最后修改时间，如：`2016-04-01 16:35:26 +0200`

`file.name`:

文件名称（带扩展名），如：文件image.jpg对应image.jpg

`file.basename`：

文件名称（不带扩展名） 如：文件image.jpg对应image

`file.extname`：

文件的扩展名，如 image.jpg 中的 .jpg

## 常用变量

Jekyll 会遍历你的网站搜寻要处理的文件。任何有 YAML 头信息的文件都是要处理的对象。对于每一个这样的文件，Jekyll 都会通过 Liquid 模板工具来生成一系列的数据。下面就是这些可用数据变量的参考和文档。

### 全局(Global)变量

`site`:

来自`_config.yml`文件，全站范围的信息+配置。

`page`:

页面专属的信息 + YAML 头文件信息。通过 YAML 头文件自定义的信息都可以在这里被获取。详情请参考下文。

`layout`：

Layout specific information + the YAML front matter. Custom variables set via the YAML Front Matter in layouts will be available here.

`content`：

被 layout 包裹的那些 Post 或者 Page 渲染生成的内容。但是又没定义在 Post 或者 Page 文件中的变量。

`paginator`：

每当 `paginate` 配置选项被设置了的时候，这个变量就可用了

### 全站(site)变量

`site.time`：

当前时间（运行jekyll这个命令的时间点）。

`site.pages`：

所有 Pages 的清单。

`site.posts`：

一个按照时间倒序的所有 Posts 的清单。

`site.related_posts`：

如果当前被处理的页面是一个 Post，这个变量就会包含最多10个相关的 Post。默认的情况下，相关性是低质量的，但是能被很快的计算出来。如果你需要高相关性，就要消耗更多的时间来计算。用 jekyll 这个命令带上 `--lsi (latent semantic indexing)` 选项来计算高相关性的 Post。注意，GitHub 在生成站点时不支持　lsi。

`site.static_files`：

静态文件的列表 (此外的文件不会被 Jekyll 和 Liquid 处理。)。每个文件都具有三个属性： `path`， `modified_time` 以及 `extname`。

`site.html_pages`：

‘site.pages’的子集，存储以‘.html’结尾的部分。

`site.html_files`：

‘site.static_files’的子集，存储以‘.html’结尾的部分。

`site.collections`：

一个所有集合（collection）的清单。

`site.data`：

一个存储了 _data 目录下的YAML文件数据的清单。

`site.documents`：

每一个集合（collection）中的全部文件的清单。

`site.categories.CATEGORY`：

所有的在 CATEGORY 类别下的帖子。

`site.tags.TAG`：

所有的在 TAG 标签下的帖子。

`site.[CONFIGURATION_DATA]`：

所有的通过命令行和`_config.yml` 设置的变量都会存到这个 `site` 里面。 举例来说，如果你设置了 `url: http://mysite.com` 在你的配置文件中，那么在你的 `Posts` 和 `Page`s 里面，这个变量就被存储在了 `site.url`。Jekyll 并不会把对 `_config.yml` 做的改动放到 `watch` 模式，所以你每次都要重启 Jekyll 来让你的变动生效。


### 页面(page)变量

`page.content`:

页面内容的源码。

`page.title`:

页面的标题。

`page.excerpt`:

页面摘要的源码。

`page.url`:

帖子以斜线打头的相对路径，例子： /2008/12/14/my-post.html。

`page.date`:

帖子的日期。日期的可以在帖子的头信息中通过用以下格式 `YYYY-MM-DD HH:MM:SS` (假设是 UTC), 或者 `YYYY-MM-DD HH:MM:SS +/-TTTT` ( 用于声明不同于 UTC 的时区， 比如 2008-12-14 10:30:00 +0900) 来显示声明其他 日期/时间 的方式被改写

`page.id`:

帖子的唯一标识码（在RSS源里非常有用），比如 `/2008/12/14/my-post`

`page.categories`:

这个帖子所属的 Categories。Categories 是从这个帖子的 _posts 以上 的目录结构中提取的。举例来说, 一个在 `/work/code/_posts/2008-12-24-closures.md` 目录下的 Post，这个属性就会被设置成 `['work', 'code']`。不过 Categories 也能在 YAML 头文件信息 中被设置。

`page.tags`:

这个 Post 所属的所有 tags。Tags 是在YAML 头文件信息中被定义的。

`page.path`:

Post 或者 Page 的源文件地址。举例来说，一个页面在 GitHub 上的源文件地址。 这可以在 YAML 头文件信息 中被改写。

`page.next`:

当前文章在`site.posts`中的位置对应的下一篇文章。若当前文章为最后一篇文章，返回`nil`

`page.previous`:

当前文章在`site.posts`中的位置对应的上一篇文章。若当前文章为第一篇文章，返回`nil`

### 分页器(Paginator)

`paginator.per_page`  每一页 Posts 的数量。

`paginator.posts` 这一页可用的 Posts。

`paginator.total_posts` Posts 的总数。

`paginator.total_pages` Pages 的总数。

`paginator.page` 当前页号。

`paginator.previous_page` 前一页的页号。

`paginator.previous_page_path` 前一页的地址。

`paginator.next_page` 下一页的页号。

`paginator.next_page_path` 下一页的地址。




