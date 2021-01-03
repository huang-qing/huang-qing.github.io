## 开发环境配置

安装:

- `python`
- `Ruby` [Ruby 使用 Ruby+Devkit 2.4.5-1 (x64)不要安装>2.5 的版本](https://rubyinstaller.org/downloads/)
- `Node`

安装插件包：

```
npm install
```

调整`gem`镜像:[ruby-china](https://gems.ruby-china.com/)

```
$ gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
$ gem sources -l 
https://gems.ruby-china.com
```

安装`jekyll`：[jekyll](https://jekyllrb.com/)

```
gem install jekyll bundler
```

初始化:

```
bundle install
```

运行开发环境：

```
bundle exec jekyll serve
```


