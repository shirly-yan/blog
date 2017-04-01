---
title: ruby属性
date: 2017-02-24 11:19:26
categories: [ruby]
tags: [ruby]
---
ruby中attr_accessor，attr_reader，attr_writer
<!-- more -->
<h3>attr_accessor</h3>
```ruby
  attr_accessor :x
  def x
    @x
  end

  def x=(x)
    @x = x
  end
```
<h3>attr_reader</h3>
```ruby
  attr_reader :x
  def x
    @x
  end
```
<h3>attr_writer</h3>
```ruby
  attr_writer :x
  def x=(x)
    @x = x
  end
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
