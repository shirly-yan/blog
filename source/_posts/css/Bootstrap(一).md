---
title: Bootstrap(一)
date: 2017-05-24 10:53:15
categories: [css]
tags: [bootstrap]
---
Bootstrap中标题 文本
<!-- more -->
<h1>标题</h1>

<h2>主标题</h2>
<img src="/images/6.png" width="800" height="349" />
```html
<h1>Bootstrap标题一</h1>
<h2>Bootstrap标题二</h2>
<h3>Bootstrap标题三</h3>
<h4>Bootstrap标题四</h4>
<h5>Bootstrap标题五</h5>
<h6>Bootstrap标题六</h6>

<div class="h1">Bootstrap标题一</div>
<div class="h2">Bootstrap标题二</div>
<div class="h3">Bootstrap标题三</div>
<div class="h4">Bootstrap标题四</div>
<div class="h5">Bootstrap标题五</div>
<div class="h6">Bootstrap标题六</div>
```

<h2>副标题</h2>
```html
<h1>Bootstrap标题一<small>我是副标题</small></h1>
<h2>Bootstrap标题二<small>我是副标题</small></h2>
<h3>Bootstrap标题三<small>我是副标题</small></h3>
<h4>Bootstrap标题四<small>我是副标题</small></h4>
<h5>Bootstrap标题五<small>我是副标题</small></h5>
<h6>Bootstrap标题六<small>我是副标题</small></h6>
```
<h1>文本</h1>

<h2>段落</h2>

+ 1、全局文本字号为14px(font-size)
+ 2、行高为1.42857143（line-height），大约是20px(大家看到一串的小数或许会有疑惑，其实他是通过LESS编译器计算出来的，当然Sass也有这样的功能)
+ 3、颜色为深灰色（#333）
+ 4、字体为"Helvetica Neue", Helvetica, Arial, sans-serif;（font-family）
```html
<p>我是一个段落，你猜我在Bootstrap是以什么样的风格显示。</p>
```

<h2>强调内容</h2>
添加类名“.lead”实现
```html
<p>第一个是普通的内容：“我是一个普通的段落，我不需要强调显示”。</p>
<p class="lead">
第二个是强调的内容：“这部分内容需要特别的强调，我和别人长得不一样”。</p>
```

<h2>粗体</h2>
可以使用<b>和<strong>标签让文本直接加粗
```html
<p>我在学习<strong>Bootstrap</strong>，我要掌握<strong>Bootstrap</strong>的所有知识。</p>
```

<h2>斜体</h2>
使用标签<em>或<i>
```html
<p>我正在学习<em>Bootstrap</em>。我发现<i>Bootstrap</i>真的好强大。</p>
```

<h2>强调类</h2>
+ .text-muted：提示，使用浅灰色（#999）
+ .text-primary：主要，使用蓝色（#428bca）
+ .text-success：成功，使用浅绿色(#3c763d)
+ .text-info：通知信息，使用浅蓝色（#31708f）
+ .text-warning：警告，使用黄色（#8a6d3b）
+ .text-danger：危险，使用褐色（#a94442）

```html
<div class="text-muted">.text-muted 效果</div>
<div class="text-primary">.text-primary效果</div>
<div class="text-success">.text-success效果</div>
<h2 class="text-info">.text-info效果</h2>
<h1 class="text-warning">.text-warning效果</h1>
<span class="text-danger">.text-danger效果</span>
<p class="text-danger">我是一段危险信息，请用Bootstrap框架中的危险风格显示</p>
```

<h2>对齐风格</h2>
Bootstrap通过定义四个类名来控制文本的对齐风格：
+ .text-left：左对齐
+ .text-center：居中对齐
+ .text-right：右对齐
+ .text-justify：两端对齐
```html
<p class="text-left">我居左</p>
<p class="text-center">我居中</p>
<p class="text-right">我居右</p>
<p class="text-justify">两端对齐</p>
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
