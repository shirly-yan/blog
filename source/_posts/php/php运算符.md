---
title: php运算符
date: 2016-10-12 17:13:21
categories: [php]
---
php算数运算符 赋值运算符 比较运算符
<!-- more -->

<h2>算数运算符</h2>
<img src="/images/7.png" width="530" height="240" />

<h2>赋值运算符</h2>
PHP的赋值运算符有两种，分别是：
+ =：把右边表达式的值赋给左边的运算数。它将右边表达式值复制一份，交给左边的运算数。换而言之，首先给左边的运算数申请了一块内存，然后把复制的值放到这个内存中。
+ &amp;：引用赋值，意味着两个变量都指向同一个数据。它将使两个变量共享一块内存，如果这个内存存储的数据变了，那么两个变量的值都会发生变化。

```php
<?php 
    $a = "我在慕课网学习PHP！";
	$b = $a;
    $c = &$a;
	$a = "我天天在慕课网学习PHP！";
	echo $b."<br />";
	echo $c."<br />";
?>

结果
我在慕课网学习PHP！
我天天在慕课网学习PHP！
```
<h2>比较运算符</h2>
<img src="/images/8.png" width="660" height="409" />

```php
<?php  
    $a = 1;
    $b = "1";
	var_dump($a == $b);
	echo "<br />";
	var_dump($a === $b);
	echo "<br />";
	var_dump($a != $b);
	echo "<br />";
	var_dump($a <> $b);
	echo "<br />";
	var_dump($a !== $b);
	echo "<br />";
	var_dump($a < $b);
	echo "<br />";
    echo "<br />111<br />";
	$c = 5;
	var_dump($a < $c);
	echo "<br />";
	var_dump($a > $c);
	echo "<br />";
	var_dump($a <= $c);
	echo "<br />";
	var_dump($a >= $b);
	echo "<br />";
	var_dump($a >= $b);
	echo "<br />";
?>

结果
bool(true) 
bool(false) 
bool(false) 
bool(false) 
bool(true) 
bool(false) 

111
bool(true) 
bool(false) 
bool(true) 
bool(true) 
bool(true) 
```

<h2>三元运算符</h2>
(“?:”)三元运算符也是一个比较运算符，对于表达式(expr1)?(expr2):(expr3)，如果expr1的值为true，则此表达式的值为expr2，否则为expr3。


```php
<?php 
    $a = 78;//成绩
	$b = $a >= 60 ? "及格": "不及格"; 
	echo $b;
?>

结果
及格
```

<h2>逻辑运算符</h2>
+ 逻辑与：都为真 则为真
+ 逻辑或：有一个为真 则为真
+ 逻辑非：真为假 假为真
+ 逻辑异或：有且只有一个为真 则为真


```php
<?php 
    $a = TRUE; 
    $b = TRUE; 
	$c = FALSE; 
	$d = FALSE; 
	echo ($a and $b)?"通过":"不通过";
	echo "<br />";
	echo($a or $c)?"通过":"不通过";
	echo "<br />";
	echo ($a xor $c xor $d)?"通过":"不通过";
	echo "<br />";
	echo !$c?"通过":"不通过";
	echo "<br />";
    echo $a && $d?"通过":"不通过";
	echo "<br />";
	echo $b || $c || $d?"通过":"不通过";
	
?>

结果
通过
通过
通过
通过
不通过
通过

```

<h2>字符串连接运算符</h2>
字符串连接运算符是为了将两个字符串进行连接，PHP中提供的字符串连
+ 连接运算符<font color=#FF6666>.</font>：它返回将右参数附加到左参数后面所得的字符串。
+ 连接赋值运算符<font color=#FF6666>.=</font>：它将右边参数附加到左边的参数后。

```php
<?php 
    $a = "张先生";
	$tip = $a.",欢迎您在慕课网学习PHP！";
	
    $b = "东边日出西边雨";	
    $b .= ",道是无晴却有晴";
    
	$c = "东边日出西边雨";	
    $c = $c.",道是无晴却有晴";
    
	echo  $tip."<br />";
	echo  $b."<br />";
	echo  $c."<br />";
?>

结果
张先生,欢迎您在慕课网学习PHP！
东边日出西边雨,道是无晴却有晴
东边日出西边雨,道是无晴却有晴
```

<h2>错误控制运算符</h2>
PHP中提供了一个错误控制运算符“@”，对于一些可能会在运行过程中出错的表达式时，我们不希望出错的时候给客户显示错误信息，这样对用户不友好。于是，可以将@放置在一个PHP表达式之前，该表达式可能产生的任何错误信息都被忽略掉；
如果激活了track_error（这个玩意在php.ini中设置）特性，表达式所产生的任何错误信息都被存放在变量$php_errormsg中，此变量在每次出错时都会被覆盖，所以如果想用它的话必须尽早检查。
需要注意的是：错误控制前缀“@”不会屏蔽解析错误的信息，不能把它放在函数或类的定义之前，也不能用于条件结构例如if和foreach等。

```php
<?php  
 $conn = @mysql_connect("localhost","username","password");
 echo "出错了，错误原因是：".$php_errormsg;
?>
```





<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
