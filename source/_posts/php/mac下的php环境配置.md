---
title: mac下的php环境配置
date: 2016-10-10 14:08:56
categories: [php]
---
mac下配置php环境的方法注意事项
<!-- more -->

服务器 Apache
----

Mac OS自带Apache，只需要启动Apache就行。
打开终端，输入命令：sudo apachectl start
打开浏览器，在地址栏中输入localhost或者127.0.0.1，出现It Works字符串，就说明Apache已经成功启动
<img src="/images/2.png" width="500" height="200" /> 

Apache的常用命令
启动Apache服务
sudo apachectl start
重启Apache服务
sudo apachectl restart
停止Apache服务
sudo apachectl stop
查看Apache版本
httpd -v
![](/images/1.png)  

Apache的网站服务器根目录在/Library/WebServer/Documents路径下


配置PHP
----

Mac OS 同样自带PHP，只需要在Apache的配置文件中添加Apache对PHP的支持就好了

打开apache的配置文件
/etc/apache2/httpd.conf

前往指定文件夹的快捷键
command + shift + g

去掉此行的注释
LoadModule php5_module libexec/apache2/libphp5.so
然后保存
![](/images/3.png)  
重启Apache服务
sudo apachectl restart

创建test.php 放入Apache的网站服务器根目录/Library/WebServer/Documents路径下
<img src="/images/4.png" width="300" height="300" /> 

<img src="/images/5.png" width="842" height="230" /> 














