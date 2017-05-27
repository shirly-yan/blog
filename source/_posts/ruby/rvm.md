---
title: rvm
date: 2017-02-04 13:40:59
categories: [ruby]
tags: [rvm] 
---
rvm
<!-- more -->

<h2>Ruby 的安装与切换</h2>

列出已知的 Ruby 版本
```ruby
rvm list known
```

安装一个 Ruby 版本
```ruby
rvm install 2.2.0 --disable-binary
```

这里安装了最新的 2.2.0, rvm list known 列表里面的都可以拿来安装。

切换 Ruby 版本
```ruby
rvm use 2.2.0
```

如果想设置为默认版本，这样一来以后新打开的控制台默认的 Ruby 就是这个版本
```ruby
rvm use 2.2.0 --default
```
查询已经安装的ruby
```ruby
rvm list
```

卸载一个已安装版本
```ruby
rvm remove 1.8.7
```
<h2>gemset</h2>
列出当前 Ruby 的 gemset
```ruby
rvm gemset list
```

建立 gemset
```ruby
rvm use 1.8.7
rvm gemset create rails23
```

删除一个 gemset
```ruby
rvm gemset delete rails2-3
```

use 可以用来切换语言或者 gemset
前提是他们已经被安装(或者建立)。并可以在 list 命令中看到。
然后所有安装的 Gem 都是安装在这个 gemset 之下。
```ruby
rvm use 1.8.7
rvm use 1.8.7@rails23
```

清空 gemset 中的 Gem
如果你想清空一个 gemset 的所有 Gem, 想重新安装所有 Gem，可以这样
```ruby
rvm gemset empty 1.8.7@rails23
```

<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
