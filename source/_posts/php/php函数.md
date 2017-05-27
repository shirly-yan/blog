---
title: php函数
date: 2016-10-14 11:44:24
categories: [php]
---
php函数
<!-- more -->
PHP内置了超过1000个函数，因此函数使得PHP成为一门非常强大的语言。大多数时候我们j使用系统的内置函数就可以满足需求，但是自定义函数通过将一组代码封装起来，使代码进行复用，程序结构与逻辑更加清晰。

<h2>自定义函数</h2>
PHP函数的定义方式：
+ 1.使用关键字“function”开始
+ 2.函数名可以是字母或下划线开头：function name()
+ 3.在大括号中编写函数体：

```php
<?php
function say()
{
    echo 'hello world';
}
//在这里调用函数
say();
?>

<?php
function sum($a, $b) {
    echo $a + $b;
}
//在这里调用函数计算1+2的值
sum(1,2);
?>

<?php
function sum($a, $b) {
    return $a+$b;
}
//在这里调用函数取得返回值
$s = sum(1,2);
print_r($s);
?>
```

<h3>可变函数</h3>
所谓可变函数，即通过变量的值来调用函数，因为变量的值是可变的，所以可以通过改变一个变量的值来实现调用不同的函数。经常会用在回调函数、函数列表，或者根据动态参数来调用不同的函数。可变函数的调用方法为变量名加括号。

```php
<?php
function func() {
    echo 'my function called.';
}
$name = 'func';
//调用可变函数

$name();
?>
```

<h2>内置函数</h2>
内置函数指的是PHP默认支持的函数，PHP内置了很多标准的常用的处理函数，包括字符串处理、数组函数、文件处理、session与cookie处理等。
另外一些函数是通过其他扩展来支持的，比如mysql数据库处理函数，GD图像处理函数，邮件处理函数等，PHP默认加载了一些常用的扩展库，我们可以安装或者加载其他扩展库来增加PHP的处理函数。

```php
<?php
$str = '苹果很好吃。';
//请将变量$str中的苹果替换成香蕉
$str = str_replace('苹果','香蕉',$str);
?>
```

<h2>判断函数是否存在</h2>
为了确保程序调用的函数是存在的，先使用function_exists判断一下函数是否存在。同样的method_exists可以用来检测类的方法是否存在。

```php
<?php
function func() {
    echo 'exists';
}
$name = 'func';
if (function_exists('func')) { //判断函数是否存在
    $name();
}
```













<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
