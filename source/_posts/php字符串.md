---
title: php字符串
date: 2016-10-14 17:19:35
categories: php
---

<!-- more -->

<h2>字符串的定义</h2>
一个字符串 通过下面的3种方法来定义：
1、单引号
2、双引号
3、heredoc语法结构

```php
//单引号定义的字符串
$hello = 'hello world';
//双引号定义的字符串
$hello = "hello world";
//heredoc语法结构定义的字符串
$hello = <<<TAG
hello world
TAG;
```

PHP允许我们在双引号串中直接包含字串变量。
而单引号串中的内容总被认为是普通字符。
比如：

```php
$str='hello';
echo "str is $str"; //运行结果: str is hello
echo 'str is $str'; //运行结果: str is $str
```

<h2>字符串的连接</h2>
PHP中用英文的点号.来连接两个字符串。

```php
$hello='hello';

$world=' world';

$hi = $hello.$world;

echo $hi;//我们可以用echo函数输出一下这个字符串连接。
```

<h2>去除字符串首尾的空格</h2>
PHP中有三个函数可以去掉字符串的空格
+ trim去除一个字符串两端空格。
+ rtrim是去除一个字符串右部空格，其中的r是right的缩写。
+ ltrim是去除一个字符串左部空格，其中的l是left的缩写。
 
例子如下：

```php
echo trim(" 空格 ")."<br>";
echo rtrim(" 空格 ")."<br>";
echo ltrim(" 空格 ")."<br>";
```

<h2>获取字符串的长度</h2>
函数strlen()

```php
$str = 'hello';
$len = strlen($str);
echo $len;//输出结果是5
```
strlen函数对于计算英文字符是非常的擅长，但是如果有中文汉字，要计算长度该怎么办？
可以使用mb_strlen()函数获取字符串中中文长度。
例子如下：

```php
$str = "我爱你";
echo mb_strlen($str,"UTF8");//结果：3，此处的UTF8表示中文编码是UTF8格式，中文一般采用UTF8编码
```

<h2>字符串的截取</h2>













<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
