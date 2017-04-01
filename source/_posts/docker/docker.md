---
title: docker
date: 2016-11-15 13:52:31
categories:
---
docker
<!-- more -->
docker是一个容器引擎，提供了一套完整的容器解决方案
基于linux容器技术，操作系统级别的虚拟化，依赖于linux内核的Namespace和Cgroups

[Docker官网](http://www.docker.com)
<h3>安装docker</h3>
安装
```markdown
# apt-get install curl

# curl -sSL https://get.docker.com/ | sh

# usermod -aG docker deploy

```
查看信息
```markdown

# docker version

# ps axf |grep docker
```

<h3>docker命令</h3>
停止
```markdown
service docker stop
```
启动
```markdown
service docker start
```
运行容器
```markdown
docker run 
```

<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
