---
title: gem
date: 2016-11-17 22:14:05
categories: ruby
---
gem常用命令
<!-- more -->
<h3>gem源</h3>
查看当前gem源
```markdown
gem sources --l
```
移除gem源
```markdown
gem sources --remove url
```
添加gem源
```markdown
gem sources -a url
```
可以用 Bundler 的 Gem 源代码镜像命令。这样不用改Gemfile 的 source。
```markdown
bundle config mirror.https://rubygems.org https://gems.ruby-china.org
```
<h3>gem常用命令</h3>
安装gem包
```markdown
gem install name
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
