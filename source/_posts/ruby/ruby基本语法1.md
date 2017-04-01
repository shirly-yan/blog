---
title: ruby基本语法1
date: 2016-03-11 17:16:29
categories: ruby
tags: [ruby]
---
ruby基本语法1
<!-- more -->
<h2>运行ruby的方式</h2>
+ 交互式

1、使用ruby命令
irb
```narkdown
shirlydeMacBook-Pro:~ shirly$ irb
2.3.0 :001 > puts "hello world"
hello world
 => nil
```

+ 编写程序文件

1、将程序保存在文件中，使用ruby命令运行

<h2>ruby文档系统</h2>
RDoc:ruby文档生成工具,通过分析ruby的代码，为ruby的对象和方法生成结构化的页面 
ri:命令行工具，方便的在控制台中查询API

生成本地文档网站
```
ri --server
```

<h2>ruby常用命令</h2>
+ 运行ruby文件 
```
ruby filename
```
+ 获取对象id的方法 object_id
```
2.3.0 :001 > "hello world".object_id
 => 70123608252540
```

<h2>ruby面向对象</h2>
+ ruby中的一切都是对象（字符串，布尔值等都是对象）
+ 每一个对象都有唯一的id（使用object_id方法获得）

<h3>定义类</h3>
```ruby
class Name
  #其余代码
end
```
<h3>局部变量，实例变量，全局变量和类变量</h3>
+ 局部变量必须以小写字母或者下划线开头
+ 实例变量以@开头
+ 类变量以@@开头
+ 全局变量以$开头

<h3>类方法，实例方法</h3>
+ 类方法 
+ 实例方法

```ruby
#全局变量
$prefix = "hello:"

#定义类
class Name
  #类变量
  @@count = 0

  def initialize(firstName, lastName)
    #实例变量
    @firstName = firstName
    @lastName = lastName
    @@count = @@count + 1
  end

  #类方法
  def self.get_name_count
    @@count
  end

  #实例方法
  def lastName
    @lastName
  end

  def firstName
    @firstName
  end

  def name_with_prefix
    $prefix + fullName
  end

  def fullName
    #局部变量
    name = @firstName + " " + @lastName
    #返回局部变量
    name
  end

end
```

<h2>ruby中的基本类型</h2>
通常所说的基本类型是不可实例化的，可以直接初始化、赋值、运算，不可调用方法的类型

<h3>符号 Symbol</h3>
一个相同的符号有唯一确定的object_id

通过
+ :name
+ :"string"
+ :to_sym
方法生成

<h3>布尔值 TrueClass FalseClass</h3>
+ 与(&) 全真为真
+ 或(|) 一真为真
+ 异或(^) 一样false不一样true

<h3>字符串</h3>
输出字符串
+ p
+ puts
+ print
```ruby
p "我是字符串" #我是字符串
p "我是字符串" * 2 #我是字符串我是字符串
p "我是" + "字符串" #我是字符串
"a".ord #97
"a" < "b" #true
"a" =~ /a/ #true
```
<h3>数字</h3>
+ Numeric 所有数字的父类
+ Fixnum  一个原生的机器字节(32位或64位)所能存储的最大的整形值
+ Bignum
+ Float
+ BigDeimal

```ruby
require 'bigdecimal'

a = BigDecimal.new("0.07")
b = a * 100
b.to_i
```

<h4>迭代</h4>
```ruby
5.times {
  puts "hehe"
}

5.times { 
|number| 
puts number
}

1.upto(10) {
|number| 
puts "#{number}"
}

10.downto(1) {
|x| 
puts x
}

5.step(50,5){
  |x|
  puts x
}
```


<h3>数组</h3>
数组是有序的，基于整数索引的任意对象的集合

<h4>索引</h4>
+ 索引的起始为0
+ 索引为负数时,从末尾开始查找，如索引为-1代表数组最后一个元素，索引为-2代表数组倒数第二个元素

<h4>创建数组</h4>
```ruby
arr = [1,"two",3.0] #[1,"two",3.0]

arr = Array.new    #[]
```

<h4>获取数组元素</h4>
```ruby
arr = [1,2,3,4,5,6] 

 #获取某个元素
arr[2] #3
arr[100] #nil
arr[-3] #4
arr.at(0) #1
arr.fetch(1) #2

 #获取摸个范围的元素
arr[2,3] #[3,4,5]
arr[1..4] #[2,3,4,5]
arr[1..-3] #[2,3,4]


 #获取前n个元素
arr.take(3) #[1,2,3] 

#排除前n个元素
arr.drop(3) #[1,2,3] 
```
<h4>获取数组信息</h4>
```ruby
arr = [1,2,3,4,5,6] 

arr.length  #6
arr.count   #6
arr.empty?  #false
arr.include?(6) #true
```
<h4>修改数组</h4>
```ruby
arr = [1,2,3,4]

 #插入元素
 
 #在末尾插入元素
arr.push(5)    #[1,2,3,4,5] 
arr<<6         #[1,2,3,4,5,6]
 #在开头插入元素
arr.unshift(0) #[0,1,2,3,4,5]
 #在特定位置插入元素
arr.insert(3,'apple')  #[0,1,2,'apple',3,4,5]

 #删减元素
arr = [1,2,3,4,5,6] 

 #删除最后一个元素
arr.pop     #6 [1,2,3,4,5] 
 #删除第一个元素
arr.shift   #1 [2,3,4,5] 
 #删除特定位置元素
arr.delete_at(2)  #4 [2,3,5] 
 #删除特定元素
arr.delete(2)  #2 [3,5] 


arr = [1,1,2,3,3,3,nil]
 #删除重复元素
arr.uniq  #[1,2,3,nil]
 #删除空元素
arr.compact #[1,1,2,3,3,3]
```
<h4>遍历数组</h4>
```ruby
arr = [1,2,3,4,5,6] 

 #遍历
arr.each
 #反向遍历
arr.reverse_each
 #对数组元素做处理后返回完整的数组 不修改arr自己
arr.map
 #选出特定的元素组成新的数组 不修改arr自己
arr.select
 #剔除选择的元素后的数组 不修改arr自己
arr.reject
 #抛弃满足条件的元素前面的元素 不修改arr自己
arr.drop_while
 #删除满足条件的元素 修改arr自己
arr.delete_if
 #保留满足条件的元素 修改arr自己
arr.keep_if
```
<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->




















