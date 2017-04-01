---
title: ruby正则
date: 2016-03-14 11:00:05
categories: ruby
tags: [ruby,rails]
---
ruby正则
<!-- more -->

<h2>新建正则</h2>
```ruby
a = Regexp.new('abcd') 
b = /abcd/
c = %r{abcd}
```

<h2>匹配正则</h2>
```ruby
a =~ "abcdefg"

a =~ "abcdefg" #0
a.match "abcdefg" #<MatchData "abcd">
```
<h2>正则修饰符</h2>
用于控制匹配结果的特殊字符
+ i 忽略大小写
+ m 匹配多行
+ x 多行编辑+注释模式
+ u e s n 控制编码

[http://regexr.com](http://regexr.com)




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
