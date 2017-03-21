---
title: rake任务
date: 2016-12-20 20:59:13
categories: ruby
tags: [ruby,rake]
---
rails中创建使用rake任务
<!-- more -->
转自[http://xiaocuixt.github.io/rails/2016/04/18/rails中创建rake任务/?utm_source=tuicool&utm_medium=referral](http://xiaocuixt.github.io/rails/2016/04/18/rails中创建rake任务/?utm_source=tuicool&utm_medium=referral)

在rails中，rake用来编写任务脚本，让我们在cli中可以执行，提供了良好的任务编写结构，并且方便执行相互依赖的任务

rails项目中，rake文件存放位置：lib/tasks/user.rake

<h3>>1.without namespace:</h3>
```ruby
task :sayhi do
  puts "hi"
end
执行 rake sayhi 即可
```
<h3>2.with namespace</h3>
```ruby
namespace :user do
   desc "genertate users"    #描述性文字
   task :generate => :environment do   #environment表示可指定环境 RAILS_ENV=production rake user:generate
       u = User.new([{name: "tom"},{name: "jack"}])
       u.save
   end
end
```
这样直接运行rake user:generate 即可执行生成user数据的任务

<h3>3.调用外部的rake方法</h3>

#lib/tasks/hello.rake
```ruby
namespace :say do
  task :hello do
    puts "hello xiaocui"
  end
end
```
#lib/tasks/hi.rake
```ruby
namespace :say do
  task :hi do
    Rake::Task['say:hello'].invoke   #调用外部的task
    puts "hi xiaocui"
  end
end
```
执行 rake say:hi 将打印出 “hello xiaocui” 和 “hi xiaocui”

<h3>4.单次执行多个方法</h3>

#lib/myweekend.rake
```ruby
task :morning do
  puts "get up"
end
task :afternoon do
  puts "go shopping"
end
task :evening do
  puts "have supper"
end
task :my_weekend => [:morning, :afternoon, :evening] do
   puts "This is my whole day on my weekend"
end
```
执行rake my_weekend 将依次打印每个方法的内容:

get up
go shopping
have supper
This is my whole day on my weekend
<h3>5.rake支持默认的任务</h3>
```ruby
task :default => 'say:hello'
```
执行rake 即可运行该任务，很少用。

﻿适用场景:

1.调整线上的数据,执行rake脚本可以修改production数据库数据

2.创建开发用的测试数据

<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
