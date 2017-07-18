---
title: File
date: 2017-04-01 20:47:18
categories: [ruby]
tags: [ruby,file]
---

<!-- more -->
Use:
```ruby
File.open("out.txt", [your-option-string]) {|f| f.write("write your stuff here") }
```
```ruby
out_file = File.new("out.txt", "w")
out_file.puts("write your stuff here")
out_file.close
```
where your options are:
+ r - Read only. The file must exist.
+ w - Create an empty file for writing.
+ a - Append to a file.The file is created if it does not exist.
+ r+ - Open a file for update both reading and writing. The file must exist.
+ w+ - Create an empty file for both reading and writing.
+ a+ - Open a file for reading and appending. The file is created if it does not exist.

读取
```ruby
File.open("out.txt") do |file|
  while line = file.gets
    puts line
  end
end
```
文件重命名：
```ruby
File.rename( "new_file.js", "new.js" )#原来的文件名，新的文件名
```
删除文件
```ruby
File.delete( "new.js" )#原来的文件名
```

目录操作：
```ruby
Dir.mkdir("new")#创建一个新文件夹
Dir.rmdir("new")#删除指定的文件夹
```

将一个文件拷贝到目标目标：
```ruby
require 'fileutils'
FileUtils.cp 'new.js', 'new'
```
将一个文件移动到目标目标：
```ruby
require 'fileutils'
FileUtils.mv 'new.js', 'new'
```



<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
