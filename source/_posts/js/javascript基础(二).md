---
title: javascript基础(二)
date: 2017-07-02 20:47:18
categories: [js]
tags: [js]
---

<!-- more -->
<h1>数组</h1>

```javascript
var myarray=new Array();

var myarray=new Array(8); //创建数组，存储8个数据。
 
var myarray = new Array(66,80,90,77,59);//创建数组同时赋值

var myarray = [66,80,90,77,59];//直接输入一个数组（称 “字面量数组”）
```
注意：
1.创建的新数组是空数组，没有值，如输出，则显示undefined。
2.虽然创建数组时，指定了长度，但实际上数组都是变长的，也就是说即使指定了长度为8，仍然可以将元素存储在规定长度以外。
3.数组存储的数据可以是任何类型（数字、字符、布尔值等）

```javascript
myarray.length; //获得数组myarray的长度
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
