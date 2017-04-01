---
title: bootstrap
date: 2016-11-14 16:28:22
categories: html
---
bootstrap
<!-- more -->
[bootstrap官网](http://getbootstrap.com/)
[bootstrap中文文档](http://v3.bootcss.com/)


[bootswatch官网](http://bootswatch.com)

gemfile中加
```markdown
gem 'therubyracer'
gem 'bootstrap-sass'
gem 'bootswatch-rails'
```

application.js加
```markdown
//= require bootstrap
```
改application.css为application.css.scss，内容改为
```markdown
// 示例：使用bootswatch免费主题： 'Cerulean' bootswatch
// 首先导入变量
@import "bootswatch/cerulean/variables";

// 导入bootstrap
@import "bootstrap";

// 最后导入需要的bootswatch主题
@import "bootswatch/cerulean/bootswatch";
```




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
