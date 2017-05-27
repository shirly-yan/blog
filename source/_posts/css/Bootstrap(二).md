---
title: Bootstrap(二)
date: 2017-05-24 16:53:15
categories: [css]
tags: [bootstrap]
---
Bootstrap列表 代码块
<!-- more -->
<h1>列表</h1>

Bootstrap根据平时的使用情形提供了六种形式的列表：
+ 普通列表
+ 有序列表
+ 去点列表
+ 内联列表
+ 描述列表
+ 水平描述列表

<h2>无序列表</h2>
```html
<ul>
    <li>列表项目</li>
    <li>列表项目</li>
    <li>列表项目</li>
    <li>列表项目</li>
    <li>列表项目</li>
</ul>
```

<h2>有序列表</h2>
```html
<ol>
      <li>项目列表一</li>
      <li>项目列表二</li>
      <li>项目列表三</li>
</ol>
```
<h2>去点列表</h2>
添加类名“.list-unstyled”
```html
<ol>
    <li>有序项目列表一</li>
    <li>有序项目列表二
      <ol class="list-unstyled">
        <li>有序二级项目列表一</li>
        <li>有序二级项目列表二</li>
        <li>
          <ul>
            <li>无序三级项目列表一</li>
            <li>无序三级项目列表二
            <ul class="list-unstyled">
              <li>无序四级项目列表一</li>
              <li>无序四级项目列表二</li>
            </ul>
            </li>
            <li>无序三级项目列表三</li>
          </ul>
        </li>
      </ol>
    </li>
    <li>有序项目列表三</li>
</ol>
```
<h2>内联列表</h2>
添加类名“.list-inline”
```html
<ul class="list-inline">
    <li>北京</li>
    <li>上海</li>
    <li>南京</li>
    <li>厦门</li>
</ul>
```
<h2>定义列表</h2>
```html
<dl>
    <dt>北京</dt>
    <dd>北京是中国的首都，是政治文化集中地</dd>
    <dt>上海</dt>
    <dd>上海号称东方的巴黎</dd>
</dl>
```

<h2>水平定义列表</h2>
给<dl>添加类名“.dl-horizontal”给定义列表实现水平显示效果
宽屏下的效果（屏幕大于768px）：
<img src="/images/52.png" width="800" height="107" />
当你缩小你的浏览器屏幕时，水平定义列表将回复到原始的状态。
<img src="/images/53.png" width="800" height="222" />
```html
<dl class="dl-horizontal">
    <dt>W3cplus</dt>
    <dd>一个致力于推广国内前端行业的技术博客。它以探索为己任，不断活跃在行业技术最前沿，努力提供高质量前端技术博文</dd>
    <dt>慕课网</dt>
    <dd>一个专业的，真心实意在做培训的网站</dd>
    <dt>我来测试一个标题，我来测试一个标题</dt>
    <dd>我在写一个水平定义列表的效果，我在写一个水平定义列表的效果</dd>
</dl>
```
<h1>代码</h1>

Bootstrap主要提供了三种代码风格：
+ 1、使用&lt;code&gt;&lt;/code&gt;来显示单行内联代码
+ 2、使用&lt;pre&gt;&lt;/pre&gt;来显示多行块代码
+ 3、使用&lt;kbd&gt;&lt;/kbd&gt;来显示用户输入代码
```html
<div>Bootstrap的代码风格有三种：<code>&lt;code&gt;</code>、<code>&lt;pre&gt;</code>和<code>&lt;kbd&gt;</code></div>
pre风格：
<div>
<pre>
&lt;ul&gt;
&lt;li&gt;...&lt;/li&gt;
&lt;li&gt;...&lt;/li&gt;
&lt;li&gt;...&lt;/li&gt;
&lt;/ul&gt;
</pre>
</div>
kbd风格：
<div>请输入<kbd>ctrl+c</kbd>来复制代码，然后使用<kbd>ctrl+v</kbd>来粘贴代码</div>
```

<h2>控制代码块区域最大高度为340px</h2>
在pre标签上添加类名“.pre-scrollable”
```html
<pre class="pre-scrollable">
<ol>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
    <li>....</li>
</ol>
</pre>
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
