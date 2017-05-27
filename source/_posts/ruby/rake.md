---
title: rake
date: 2016-11-14 15:23:56
categories: [ruby]
tags: [ruby,rake]
---
rake
<!-- more -->

+ rake -T 查看所有的rake任务

<h3>和数据库相关的</h3>
+ rake db:create  #创建数据库
+ rake db:migrate #更新数据库，更新的文件来自db/migrate/
+ rake db:seed    #执行seed.rb文件，通常是创建一个默认的数据
+ rake db:drop    #删除数据库

生产环境下 加前缀 RAILS_ENV=protucion 

