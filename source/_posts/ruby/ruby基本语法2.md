---
title: ruby基本语法2
date: 2016-03-13 17:40:04
categories: ruby
tags: [ruby]
---
ruby基本语法2
<!-- more -->
<h2>哈希</h2>
哈希是一种类似字典的集合，集合中包含了唯一的键和键所对应的值

<h3>新建哈希</h3>
```ruby
hsh = {"a" => 10, "b" => 6}
hsh = {a:10, b:6}
hsh = Hash.new

hsh = Hash.new(0)  #设置默认值
hsh[:key_not_exit] #0
hsh.default        #0

hsh = Hash.new{|hash, key| hash[key] = "我是hash#{key}"}
hsh[:key_not_exit] #我是hash key_not_exit
```
<h3>获取哈希信息</h3>
```ruby
hsh = {key1:1, key2:2}
 
 #获取哈希的默认初始值
hsh.default #nil
 #判断哈希是否包含键值对
hsh.empty?  #false
 #判断哈希是否相等
hsh.eql?({}) #false
 #获取哈希的键值对个数
hsh.length #2
 #获取哈希的键值对个数
hsh.size   #2
 #清除全部键值对
hsh.clear  #{}
```
<h3>访问哈希中的键值对</h3>
```ruby
hsh = {key1:1, key2:2}

hsh[:key1]  #1
hsh[:key3]  #nil
hsh.fetch(:key1)  #1
hsh.fetch(:key_not_exit)  #raise key error
 #判断哈希中是否存在键值对
hsh.has_key?(:key1)   #true
 #查询值所对应的键
hsh.key(2) #:key2
 #查询所有的键
hsh.keys #[:key1,:key2]
 #查询所有的值
hsh.values #[1,2]

hsh.assoc(:key1) #[:key1,1]
```

<h3>增加哈希中的键值对</h3>
```ruby
hsh = {key1:1, key2:2}

 #增加键值对
hsh[:key3] = 3 #3
hsh.store(:key3, 3) #{key1:1, key2:2, key3:3}
 #合并2个哈希，后面的值会覆盖前面的值 merge不修改本身 merge!需改本身
hsh.merge({key3:3}) #{key1:1, key2:2, key3:3}
hsh.update
```
<h3>删除哈希中的键值对</h3>
```ruby
hsh = {key1:1, key2:2}

 #删除对应的键值对
hsh.delete(:key2) #2
 #删除满足条件的键值对
hsh.delete_if{|key, value| value > 1} #{}
 #保留满足条件的键值对
hsh.keep_if
 #删除第一个键值对
hsh.shift

```

<h3>遍历哈希中的键值对</h3>
```ruby
hsh = {key1:1, key2:2}

hsh.each
hsh.each_pair
hsh.each_key
hsh.each_value
hsh.select
hsh.reject

hsh.each_key{|key| puts key}
```

<h2>方法</h2>
+ 方法名应该以小写字母或者下划线开头
+ 方法名可以以？、！、或者=号结尾

```ruby
def my_method (arg1, arg2)
  puts arg1, arg2
end

def my_method (arg1 = "arg1", arg2 = "arg2")
  puts arg1, arg2
end

my_method           # "arg1, arg2"
my_method("hi")          # "hi, arg2"
my_method("hi", "world") # "hi, world"

def my_method (arg1, *rest)
  puts "arg1 = #{arg1}, rest = #{rest.inspect}"
end

def my_method (first, *, last)

end

def my_method (params)

end

my_method(:arg1 => "arg1", :arg2 => "arg2")
my_method({:arg1 => "arg1", :arg2 => "arg2"})

def my_method (arg)
  if block_given?
    yield(arg)
  else
    puts "no block given"
  end
end

def my_method (arg, &block)
  block.call(arg)
end

my_method("hi"){|a| p a} 
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
