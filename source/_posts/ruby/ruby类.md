---
title: ruby类
date: 2016-04-11 19:00:05
categories: [ruby]
tags: [ruby]
---
<!-- more -->

<h2>常用命令</h2>
irb中引入ruby文件
```ruby
load 'point.rb'
```

获得一个对象的类
```ruby
p.class
```

获得一个对象的所有方法
```ruby
p.methods
```

获得一个对象的所有方法,不显示从祖先继承的方法
```ruby
p.methods(false)
```

显示一个类的祖先
```ruby
Point.ancestors
```

<h2></h2>
<h3>创建类</h3>
```ruby
class Point

end
```

<h3>默认执行的初始化方法,构造方法</h3>
```ruby
class Point
  def initialize
    puts self
  end
end
```

<h3>带参数的构造方法</h3>
```ruby
class Point
  def initialize(x, y)
    @x, @y = x, y
  end
end
```
<font color=#FF6666>方法有几个参数必须传入几个参数，除非设置了默认值的参数可以不传</font>
```ruby
class Point
  def initialize(x, y=2)
    @x, @y = x, y
  end
end
```
```ruby
class Point
  def initialize(x=0, y=0)
    @x, @y = x, y
  end
end
```


<h3>变量的类型</h3>
<h4>实例变量 instance variable</h4>
```ruby
@x
```
<h4>类变量 class variable</h4>
```ruby
@@x
```
<h4>全局变量 global variable</h4>
```ruby
$x
```
<h4>局部变量 local variable</h4>
```ruby
x
```

<h3>getter setter方法</h3>
```ruby
class Point
  attr_accessor :x # getter setter
  attr_reader :y # getter
  attr_writer :x #setter
  def initialize(x = 0, y = 0, z = 0)
    @x, @y, @z = x, y, z
    puts x, y, z
  end
end
```
<h3>符号方法</h3>
```ruby
class Point
  attr_accessor :x, :y, :z

  def initialize(x = 0, y = 0, z = 0)
    @x, @y, @z = x, y, z
  end

  def +(p2)
    Point.new(@x + p2.x, @y + p2.y, @z + p2.z)
  end

end
```
<h3>类方法</h3>
<h4>在方法前加 self.</h4>
```ruby
class Point
  attr_accessor :x, :y, :z

  def initialize(x = 0, y = 0, z = 0)
    @x, @y, @z = x, y, z
  end

  def self.first_quadrant?(point)
    point.class == Point && point.x > 0 && point.y > 0
  end
end
```




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
