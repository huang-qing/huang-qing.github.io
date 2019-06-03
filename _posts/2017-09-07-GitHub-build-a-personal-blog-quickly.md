---
layout:     post
title:      快速搭建个人博客
subtitle:   手把手教你在半小时内搭建自己的个人博客(如果不踩坑的话🙈🙊🙉)
date:       2017-09-07
author:     huangqing
header-img: img/post-bg-github-cup.jpg
catalog: true
categories: [GitHub]
tags:
    - Blog
    - GitHub Pages
    - jekyll
---


# 快速开始

### 从注册一个Github账号开始

采用的搭建博客的方式是使用 [GitHub Pages](https://pages.github.com/) + [jekyll](http://jekyll.com.cn/) 的方式。

要使用 GitHub Pages，首先你要注册一个 [GitHub](https://github.com/) 账号，GitHub 是全球最大的同性交友网站(吐槽下程序员~)，你值得拥有。

![](/images/github/006tKfTcgy1fch0a9kz7aj31kw0z7npd.jpg)

### 拉取我的博客模板

注册完成后搜索 `huang-qing.github.io` 进入[我的仓库](https://github.com/huang-qing/huang-qing.github.io)


![](/images/github/2017-09-09_110553.png)

点击右上角的 **Fork** 将我的仓库拉倒你的账号下

稍等一下，点击刷新，你会看到**Fork**了成功的页面


### 修改仓库名

点击**settings**进入设置

<p id = "Rename"></p>
修改仓库名为 `你的Github账号名.github.io`，然后 Rename

![](/images/github/2017-09-09_111456.png)

这时你在在浏览器中输入 `你的Github账号名.github.io` 例如:`huang-qing.github.io`

你将会看到如下界面

![](/images/github/2017-09-09_112022.png)

说明已经成功一半了😀。。。当然，还需要修改博客的配置才能变成你的博客。

若是出现

![](/images/github/006tNbRwgy1fcgqz6dyxmj30we0n83yy.jpg)

则需要 [检查一下你的仓库名是否正确](#Rename)

### 整个网站结构

修改Blog前我们来看看Jekyll 网站的基础结构，当然我们的网站比这个复杂。

```
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
├── _data
|   └── members.yml
├── _site
├── img
└── index.html
```

很复杂看不懂是不是，不要紧，你只要记住其中几个OK了

- `_config.yml` 全局配置文件
- `_posts`	放置博客文章的文件夹
- `img`	存放图片的文件夹

其他的想继续深究可以[看这里](http://jekyll.com.cn/docs/structure/)



### 修改博客配置

来到你的仓库，找到`_config.yml`文件,这是网站的全局配置文件。

![](/images/github/2017-09-09_113220.png)

点击修改

![](/images/github/2017-09-09_113513.png)

然后编辑`_config.yml`的内容



接下来我们来详细说说以下配置文件的内容：

#### 基础设置

```
# Site settings
title: You Blog    				  	#你博客的标题
SEOTitle: 你的博客 | You Blog    	 #显示在浏览器上搜索的时候显示的标题
header-img: img/post-bg-rwd.jpg  	#显示在首页的背景图片
email: You@gmail.com	
description: "You Blog"  			 #网站介绍
keyword: "HUANG QING, HUANG QING Blog, 黄庆的博客, Front-End" #关键词
url: "https://huang-qing.github.io"          # 这个就是填写你的博客地址
baseurl: ""      # 这个我们不用填写

```
#### 侧边栏

```
# Sidebar settings
sidebar: true                           # 是否开启侧边栏.
sidebar-about-description: "说点装逼的话。。。"
sidebar-avatar: img/avatar-huangqing.JPG      # 你的个人头像 这里你可以改成我在img文件夹中的两张备用照片 img/avatar-m 或 avatar-g
```


#### 社交账号
展示你的其他社交平台

![](/images/github/006tNbRwgy1fcgsm4plpdj307i03nt8i.jpg)

在下面你的社交账号的用户名就可以了，若没有可不用填

```
# SNS settings
RSS: false
weibo_username:     username
zhihu_username:     username
github_username:    username
facebook_username:  username
jianshu_username:	jianshu_id
```

新加入了**简书**，`jianshu_id` 在你打开你的简书主页后的地址如：`http://www.jianshu.com/u/a5f4cd1641eb`中，后面这一串数字：`a5f4cd1641eb `

#### 评论系统

待续。。。😅

#### 网站统计

集成了 [Baidu Analytics](http://tongji.baidu.com/web/welcome/login) 和 [Google Analytics](http://www.google.cn/analytics/)，到各个网站注册拿到`track_id`替换下面的就可以了


若不想启用统计，直接删除或注释掉就可以了

```
# Analytics settings
# Baidu Analytics
ba_track_id: xxxxxxxxxxxxxxxxxxxxx

# Google Analytics
ga_track_id: 'UA-xxxxxx-xx'            # Format: UA-xxxxxx-xx
ga_domain: auto
```

#### 好友

```
friends: [
    {
        title: "简书",
        href: "http://www.jianshu.com/u/a5f4cd1641eb"
    },{
        title: "知乎",
        href: "https://www.zhihu.com/"
    },{
        title: "花瓣",
        href: "http://huaban.com/"
    }
]
```

#### 保存
讲网页拉倒底部，点击 `Commit changes` 提交保存

![](/images/github/006tKfTcgy1fch1mpktilj31kw0z7n34.jpg)

再次进入你的主页（估计需要几分钟之后），恭喜你，你的个人博客搭建完成了😀。

# 写文章

利用 Github网站 ，我们可以不用学习[git](https://git-scm.com/)，就可以轻松管理自己的博客

对于轻车熟路的程序猿来说，使用git管理会更加方便。。。

## 创建
文章统一放在网站根目录下的 `_posts` 的文件夹中。

![](/images/github/2017-09-09_131755.png)

创建一个文件

![](/images/github/2017-09-09_132101.png)

在下面写文章，和标题，还能实时预览，最后提交保存就能看到自己的新文章了。

![](/images/github/2017-09-09_132201.png)


## 格式
每一篇文章文件命名采用的是`2017-02-04-Hello-2017.md`时间+标题的形式，空格用`-`替换连接。

文件的格式是 `.md` 的 **MarkDown**文件。

我们的博客文章格式采用是 **MarkDown**+ **YAML** 的方式。

[**YAML**](http://www.ruanyifeng.com/blog/2016/07/yaml.html?f=tt) 就是我们配置 `_config`文件用的语言。

[**MarkDown**](/markdown/2017/01/02/markdown-syntax/) 是一种轻量级的「标记语言」，很简单。

大概就是这么一个结构。

```
---
layout:     post   				    # 使用的布局（不需要改）
title:      My First Post 				# 标题 
subtitle:   Hello World, Hello Blog #副标题
date:       2017-02-06 				# 时间
author:     BY 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 生活
---

## Hey
>这是我的第一篇博客。

进入你的博客主页，新的文章将会出现在你的主页上.
```

按格式创建文章后，提交保存。进入你的博客主页，新的文章将会出现在你的主页上.


#### 首页标签

在首页可以看到这些特色标签，当你的文章出现相同标签（默认相同的**标签数量大于1**），才会自动生成。

所以当你只放一篇文章的时候是不会出现标签的。



![](/images/github/006tKfTcgy1fh7uy90ayzj30gd09udh0.jpg)


建站的初期，博客比较少，若你想直接在首页生成比较多的标签。你可以在 `_congfig.yml`中找到这段：

```
# Featured Tags
featured-tags: true                     # 是否使用首页标签
featured-condition-size: 1              # 相同标签数量大于这个数，才会出现在首页
```

将其修改为`featured-condition-size: 0`, 这样只有一个标签时也会出现在首页了。

相反，当你博客比较多，标签也很多时，这时你就需要改回 `1` 甚至是 `2` 了。


到这里，恭喜你！

你已经成功搭建了自己的个人博客以及学会在博客上撰写文字的技能了（是不是有点小兴奋🙈）。


# 自定义域名

搭建好博客之后 你可能不想直接使用 [huang-qing.github.io](http://huang-qing.github.io) 这么长的博客域名吧, 想换成想 [huang-qing.top](http://huang-qing.github.io) 这样简短的域名。

那我们开始吧！

#### 购买域名
首先，你必须购买一个自己的域名。（我没有购买域名，直接`Copy` BY 的博客）

我是在[阿里云](https://wanwang.aliyun.com/domain/?spm=5176.8006371.1007.dnetcndomain.q1ys4x)购买的域名

![](/images/github/006tKfTcgy1fci89zv06yj31kw11p1kx.jpg)

用**阿里云** app也可以注册域名，域名的价格根据后缀的不同和域名的长度而分，比如我这个 `huang-qing.top` 的域名第一年才只要4元~

域名尽量选择短一点比较好记住，注意，不能选择中文域名，比如 `张三.top` ,GitHub Pages **无法处理中文域名**，会导致你的域名在你的主页上使用。

注册的步骤就不在介绍了

#### 解析域名

注册好域名后，需要将域名解析到你的博客上

管理控制台 → 域名与网站（万网） → 域名

![](/images/github/006tKfTcgy1fci8phk5z9j30nk0q0goy.jpg)

选择你注册好的域名，点击解析

![](/images/github/006tKfTcgy1fci8sg27bfj31kw0s0qdt.jpg)

添加解析

分别添加两个`A` 记录类型,

一个主机记录为 `www`,代表可以解析 `www.huang-qing.top`的域名

另一个为 `@`, 代表 `huang-qing.top`

记录值就是我们博客的IP地址，是 GitHub Pagas 在美国的服务器的地址 `151.101.100.133`

![](/images/github/006tKfTcgy1fci8x9412oj31kw0o4n5o.jpg)


可以通过 [这个网站](http://ip.chinaz.com/)  或者直接在终端输入`ping 你的地址`，查看博客的IP

	ping huang-qing.github.io

细心地你会发现所有人的博客都解析到 `151.101.100.133` 这个IP。

然后 GitHub Pages 再通过 `CNAME`记录 跳转到你的主页上。


#### 修改CNAME

最后一步，只需要修改 我们github仓库下的 **CNAME** 文件。

选择 **CNAME** 文件

![](/images/github/006tKfTcgy1fci9q9ne6qj31kw0uuajm.jpg)

使用的注册的域名进行替换,然后提交保存

![](/images/github/006tKfTcgy1fci9rzk0naj316s0n841s.jpg)


这时，输入你自己的域名，就可以解析到你的主页了。

大功告成！

# 进阶

若你对博客模板进行修改，你就要看看 Jekyll 的[开发文档](http://jekyllcn.com/),是中文文档哦，对英语不好的朋友简直是福利啊（比如说我😀）。

还要学习 **Git** 和 **GitHub** 的工作机制了及使用。

你可以先看看这个[git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)，对git有个初步的了解后，那么相信你就能将自己图片传到GitHub仓库上，或者可以说掌握了 **使用git管理自己的GitHub仓库** 的技能呢。

对于轻车熟路的程序猿来说，这篇教程就算就结束了，因为下面的内容对于你们来说 so eazy~

但相信很多小白都一脸懵逼，那我们继续👇。

# 利用GithHub Desktop管理GitHub仓库

[GithHub Desktop](https://desktop.github.com/) 是 **GithHub** 推出的一款管理GitHub仓库的桌面软件，换句话说就是将你在**Github**上的文件同步到本地电脑上，并将修改后的文件同步到**Github**远程仓库。


# 常见问题

#### 配置文件修改后没有效果

刷新几遍浏览器就好了~

不行的话，先清除浏览器缓存再试试。

#### 404错误

1. 检查你的仓库名是否有按照要求填写
2. 确定 **Fork** 的是不是我的仓库~

#### 修改CNAME文件，域名还是不变

清除浏览器缓存就OK~


# 在本地调试博客

有心的同学在 [jekyll官网](http://jekyllrb.com/) 就会发现 `jekyll` 提供的实例代码。

`jekyll`需要`Ruby`和`RubyGems`支持，windows下先下载安装[RubyInstaller for windows](https://rubyinstaller.org/)。详细可参考[这里](http://jekyllrb.com/docs/windows/#installation)

我使用的编辑器是VS Code,可以在[这里](https://code.visualstudio.com/)下载。


```
# Install Jekyll and Bundler gems through RubyGems
~ $ gem install jekyll bundler

# Create a new Jekyll site at ./myblog
~ $ jekyll new myblog

# Change into your new directory
~ $ cd myblog

# Build the site on the preview server
~/myblog $ bundle exec jekyll serve

# Now browse to http://localhost:4000
```

关于`bundler` 可查看[About Bundler](http://jekyllrb.com/docs/quickstart/#about-bundler),


这段命令创建了一个默认的 `jekll` 网站，然后在本机的 4000 窗口展示。聪明的你应该发现怎么做了吧~

安装 `jekyll`和 `jekyll bundler`

```
$ gem install jekyll bundler
```

进入你的 **Blog 所在目录**，然后创建本地服务器

```
$ F:\huang-qing.github.io> bundle exec jekyll serve

```

然后会显示 

```
Configuration file: F:/huang-qing.github.io/_config.yml
            Source: F:/huang-qing.github.io
       Destination: F:/huang-qing.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 2.631 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'F:/huang-qing.github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

你就可以在 <http://127.0.0.1:4000/> 看到你的博客，你对本地博客的修改都会在这个地址进行显示，这大大提高了对博客的配置效率。

使用`ctrl+c`就可以停止 **serve**

# Star

若本教程顺利帮你搭建了自己的个人博客，请不要 **害羞**，给我的 [github仓库](https://github.com/huang-qing/huang-qing.github.io) 点个 **star** 吧😀！