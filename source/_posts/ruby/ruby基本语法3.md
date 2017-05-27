---
title: ruby基本语法3
date: 2016-03-17 17:40:04
categories: [ruby]
tags: [ruby]
---
ruby基本语法3
<!-- more -->

<h3>类的定义</h3>
```ruby
class Point
  attr_accessor :x
  attr_reader :y
  
  @@origin = 0
  ORIGIN = 2
  
  def initialize(x = 0, y = 0)
    @x, @y = x, y
  end
  
  def another_method

  end
  
  def first_quarant?
    x > 0 && y > 0
  end
  
  def +(p2)
    Point.new(x + p2.x, y + p2.y)
  end
  
  def second_quarant?(x, y)
      x < 0 && y > 0
  end
  
  class << self
    def foo
    end
    def bar
    end
    def wat
    end
  end
end
```

<h3>类的继承</h3>
```ruby
class Point3D < Point
  def initialize(x = 0, y = 0, z = 0)
    @x, @y, @z = x, y, z
  end
  
  def initialize(x = 0, y = 0, z = 0)
    super(x, y)
    @z = z
  end
    
  def initialize(x = 0, y = 0, z = 0)
    super
  end
  
end 
```

<h3>模块</h3>
+ include:mixin module instance methods as class' instance methods
+ extend:mixin module instance methods as class' class methods
```ruby
module Helper

end

class Point
  include Helper
end

class Point
  extend Helper
end

module Helper
  def shfit_right(x, y, z = 0)
  end
  
  def ClassMethods
    def distance(obj1, obj2)
      Math.sqrt((obj1.x - obj2.x)**2 + (obj1.y - obj2.y)**2)
    end
  end
  
  def self.included(klass)
    klass.extend ClassMethods
  end
end
```
```ruby
module Lib
  BUCKETS = [0, 1000, 10_000, 50_000, 100_000]
  def annual_fee
    case balance
      when BUCKETS[0]...BUCKETS[1]
        10
      when BUCKETS[1]...BUCKETS[2]
        5
      when BUCKETS[2]...BUCKETS[3]
        3
      else
        0
    end
  end
end
```
<h3>exception</h3>

```ruby
def factorial(n)
  raise TypeError unless n.is_a? Integer
  raise ArgumentError if n < 1
  return 1 if n == 1
  n * factorial(n-1)
end

begin
  x = factorial(1)
rescue ArgumentError => e
  puts 'try again with a value >= 1'
rescue TypeError => e
  puts 'try again with an integer'
else
  puts x
ensure
  puts 'the process of factorial calculation is completed'
end
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
