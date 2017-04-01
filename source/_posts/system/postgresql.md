---
title: postgresql
date: 2016-11-18 22:06:40
categories:
---
postgresql
<!-- more -->
<h3>允许远程访问</h3>
<h4>一</h4>
postgresql.conf
```markdown
修改
#listen_addresses ='localhost'为
listen_addresses ='*'
```
<h4>二</h4>
pg_hba.conf
```markdown
添加一行
host    all    all    0.0.0.0/0    md5
这样就是允许全部ip的任意访问了
```


```markdown
gem install pg -- --with-pg-config=/Applications/Postgres.app/Contents/Version‌​s/9.6/bin/pg_config
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
