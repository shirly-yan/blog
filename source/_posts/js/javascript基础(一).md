---
title: javascript基础(一)
date: 2017-07-01 20:47:18
categories: [js]
tags: [js]
---

<!-- more -->
<h1>在html中插入javascript代码</h1>

```html
<script type="text/javascript"></script>
```
script type="text/javascript">表示在<script></script>之间的是文本类型(text),javascript是为了告诉浏览器里面的文本是属于JavaScript语言。

<h1>在html中引入javascript文件</h1>

```html
<script src="script.js"></script>
```
就可将JS文件嵌入HTML文件中。

<h1>JavaScript代码放html文件中的位置</h1>

JavaScript代码可以放在html文件中任何位置
一般放在网页的head或者body部分。

<h2>放在 head 部分</h2>
最常用的方式是在页面中head部分放置 script 元素，浏览器解析head部分就会执行这个代码，然后才解析页面的其余部分。

<h2>放在 body 部分</h2>
JavaScript代码在网页读取到该语句的时候就会执行。

<h1>注释</h1>

```javascript
//单行注释
    
/*
多行注释
*/
```

<h1>变量</h1>

+ 1. 在JS中区分大小写，如变量mychar与myChar是不一样的，表示是两个变量。
+ 2. 变量虽然也可以不声明，直接使用，但不规范，需要先声明，后使用

<h2>定义变量</h2>

```javascript
var 变量名
```

<h2>变量命名规则</h2>

+ 1.变量必须使用字母、下划线(_)或者美元符($)开始。
+ 2.然后可以使用任意多个英文字母、数字、下划线(_)或者美元符($)组成。
+ 3.不能使用JavaScript关键词与JavaScript保留字。

<h1>判断语句</h1>
<h2>if...else</h2>

```javascript
if(条件)
{ 条件成立时执行的代码 }
else
{ 条件不成立时执行的代码 }
```

```html
<script type="text/javascript">
   var myage = 18;
   if(myage>=18)  //myage>=18是判断条件
   { 
     document.write("你是成年人。");
   }
   else  //否则年龄小于18
   { 
     document.write("未满18岁，你不是成年人。");
   }
</script>
```

<h1>函数</h1>

```javascript
function 函数名()
{
     函数代码;
}
```
```javascript
function add2(){
   var sum = 3 + 2;
   alert(sum);
}
```

<h1>输出内容</h1>

```javascript
document.write();

document.write(mystr+"<br>");//输出hello后，输出一个换行符
```
可用于直接向 HTML 输出流写内容。简单的说就是直接在网页中输出内容

<h1>消息对话框</h1>
<h2>alert</h2>

```javascript
alert(字符串或变量);  
```

<h2>confirm</h2>

```javascript
confirm(字符串或变量);
```

返回值: Boolean值
返回值: 当用户点击"确定"按钮时，返回true
        当用户点击"取消"按钮时，返回false
        
```javascript
    var mymessage=    confirm("你喜欢JavaScript吗?");    
    if(mymessage==true)
    {
    	document.write("你是女士!");
    }
    else
    {
        document.write("你是男士!");
    }
```

<h2>prompt</h2>

```javascript
prompt(str1, str2);
```
参数说明：
str1: 要显示在消息对话框中的文本，不可修改
str2：文本框中的内容，可以修改

返回值:
1. 点击确定按钮，文本框中的内容将作为函数返回值
2. 点击取消按钮，将返回null

<h1>窗口</h1>
<h2>打开窗口</h2>

open() 方法可以查找一个已经存在或者新建的浏览器窗口

语法：
```javascript
window.open([URL], [窗口名称], [参数字符串])

window.open('http://www.imooc.com','_blank','width=300,height=200,menubar=no,toolbar=no, status=no,scrollbars=yes')
```
参数说明:
+ URL：可选参数，在窗口中要显示网页的网址或路径。如果省略这个参数，或者它的值是空字符串，那么窗口就不显示任何文档。
+ 窗口名称：可选参数，被打开窗口的名称。
    1.该名称由字母、数字和下划线字符组成。
    2."_top"、"_blank"、"_self"具有特殊意义的名称。
       _blank：在新窗口显示目标网页
       _self：在当前窗口显示目标网页
       _top：框架网页中在上部窗口中显示目标网页
    3.相同 name 的窗口只能创建一个，要想创建多个窗口则 name 不能相同。
    4.name 不能包含有空格。
+ 参数字符串：可选参数，设置窗口参数，各参数用逗号隔开。

<h2>关闭窗口</h2>
```javascript
window.close();   //关闭本窗口

<窗口对象>.close();   //关闭指定的窗口

  var mywin=window.open('http://www.imooc.com'); //将新打的窗口对象，存储在变量mywin中
   mywin.close();
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
