---
title: php变量
date: 2016-10-12 10:42:53
categories: php
---
变量的声明 变量的数据类型
<!-- more -->

<h2>变量</h2>
<h3>变量声明</h3>
1、变量名需要<font color=#FF6666>$</font>进行标识
2、变量名必须以字母或下划线<font color=#FF6666>\_</font>开头
3、变量名只能由字母、数字、以及下划线<font color=#FF6666>\_</font>组成，
<font color=#FF6666>PHP中变量名是区分大小写的</font>

```php
<?php
$var_name = "苹果";
$n = 10;
?>
```

<h3>变量的数据类型</h3>
在php中，支持8种原始类型，其中包括四种标量类型、两种复合类型和两种特殊类型。php是一门松散类型的语言，不必向php声明变量的数据类型，php会自动把变量转换为自动的数据类型，
var_dump()函数获取变量的数据

```php
<?php 
 $string = "就";
 var_dump($string);
 echo "<br />";
 $string = 9494; 
 var_dump($string);
  echo "<br />";
?>
结果
string(3) "就" 
int(9494)
```
<h3>标量类型—布尔类型</h3>
布尔类型（boolean）：只有两个值，一个是<font color=#FF6666>TRUE</font>,另一个<font color=#FF6666>FALSE</font>,
它不区分大小写，也就是说”TRUE”和“true”效果是一样的

```php
<?php 
    $man = "男";
	$flag = $man == "男";
	echo $flag ;
	echo "<br />" ;
	var_dump($flag);
?>
结果
1
bool(true)
```
<h3>标量类型—整型</h3>
整型（integer）：类似于常见的整数。它可以用十进制、八进制、十六进制指定。十进制就是日常使用的数字；八进制，数字前必须加上<font color=#FF6666>0</font>(这个<font color=#FF6666>0</font>是阿拉伯数字<font color=#FF6666>0</font>，可不是英文字母“欧”哦)；十六进制，数字前必须加<font color=#FF6666>0x</font> (这个<font color=#FF6666>0</font>也是阿拉伯数字<font color=#FF6666>0</font>，不是“欧”哦)。如：

```php
<?php
	$data_int1 = 123;
	echo $data_int1;
	echo "<br />";
	$data_int2 = -123;
	echo $data_int2;
	echo "<br />";
	$data_int3 = 0123;
	echo $data_int3;
	echo "<br />";
	$data_int4 = 0x123;
	echo $data_int4;
	echo "<br />";
?>
结果
123
-123
83
291
```

<h3>标量类型—浮点型</h3>
浮点型（浮点数、双精度数或实数），也就是通常说的小数，可以用小数点或者科学计数法表示。科学计数法可以使用小写的e，也可以使用大写的E

```php
<?php
$num_float = 1.234;    //小数点  
$num_float = 1.2e3;    //科学计数法，小写e  
$num_float = 7.0E-10;  //科学计数法，大写E  
?>

结果
1.234
1200
0.007
```
<font color=#FF6666>e3是10的三次方，E-3是10的-3次方</font>

<h3>标量类型—字符串</h3>
字符串是由一系列字符组成，在PHP中，字符和字节一样，也就是说，一共有256种不同字符的可能性。字符串型可以用三种方法定义：单引号形式、双引号形式和Heredoc结构形式。

```php
<?php 
$str_string1 = '我是字符串'; 
$str_string2 = "我也是字符串哦";
echo $str_string1;
echo "<br />";
echo $str_string2;
?>

结果
我是字符串
我也是字符串哦
```

<h4>字符串中包括引号</h4>
单引号中嵌入双引号，直接嵌入
双引号中嵌入单引号，直接嵌入
单引号中嵌入单引号，使用转义字符<font color=#FF6666>\'</font>
双引号中嵌入双引号，使用转义字符<font color=#FF6666>\"</font>

```php
<?php 
$str_string1 = '甲问："你在哪里学的PHP？"';//单引号中嵌入双引号，直接嵌入
$str_string2 = "乙毫不犹豫地回答：'当然是慕课网咯！'";//双引号中嵌入单引号，直接嵌入
$str_string3 = '甲问:\'能告诉我网址吗？\'';//单引号中嵌入单引号，使用转义字符\'
$str_string4 = "乙答道:\"www.imooc.com\"";//双引号中嵌入双引号，使用转义字符\"
echo $str_string1;
echo "<br />";
echo $str_string2;
echo "<br />";
echo $str_string3;
echo "<br />";
echo $str_string4;
echo "<br />";
?>

结果
甲问："你在哪里学的PHP？"
乙毫不犹豫地回答：'当然是慕课网咯！'
甲问:'能告诉我网址吗？'
乙答道:"www.imooc.com"
```

<h4>当引号遇到美元符号标识的变量</h4>
当双引号中包含变量时，变量会与双引号中的内容连接在一起；
当单引号中包含变量时，变量会被当做字符串输出。

```php
<?php 
$love = "I love you!"; 
$string1 = "慕课网,$love";
$string2 = '慕课网,$love';
echo $string1;
echo "<br />";
echo $string2;
?>

结果
慕课网,I love you!
慕课网,$love
```
<h4>字符串很长</h4>
我们可以使用Heredoc结构形式
以<font color=#FF6666>&lt;&lt;&lt;GOD</font>开头
️以<font color=#FF6666>GOD</font>结尾

```php
<?php 
$string = <<<GOD
我有一只小毛驴，我从来也不骑。
有一天我心血来潮，骑着去赶集。
我手里拿着小皮鞭，我心里正得意。
不知怎么哗啦啦啦啦，我摔了一身泥.
GOD;

echo $string;
?>

结果
我有一只小毛驴，我从来也不骑。 有一天我心血来潮，骑着去赶集。 我手里拿着小皮鞭，我心里正得意。 不知怎么哗啦啦啦啦，我摔了一身泥.
```

<h3>特殊类型—资源</h3>
资源（resource）：资源是由专门的函数来建立和使用的，例如打开文件、数据连接、图形画布。我们可以对资源进行操作（创建、使用和释放）。任何资源，在不需要的时候应该被及时释放。如果我们忘记了释放资源，系统自动启用垃圾回收机制，在页面执行完毕后回收资源，以避免内存被消耗殆尽。

```php
<?php
$file=fopen("f.txt","r");   //打开文件
$con=mysql_connect("localhost","root","root");  //连接数据库
$img=imagecreate(100,100);//图形画布
?>

<?php 
//首先采用“fopen”函数打开文件，得到返回值的就是资源类型。
$file_handle = fopen("http://127.0.0.1/test.txt","r");
if ($file_handle){
    //接着采用while循环（后面语言结构语句中的循环结构会详细介绍）一行行地读取文件，然后输出每行的文字
    while (!feof($file_handle)) { //判断是否到最后一行
        $line = fgets($file_handle); //读取一行文本
        echo $line; //输出一行文本
        echo "<br />"; //换行
    }
}
fclose($file_handle);//关闭文件
?>

结果
我是第一行 
我是第二行 
我是第三行 
我是第四行 
我是第五行 
```

<h3>特殊类型—空类型</h3>
NULL（NULL）：NULL是空类型，对大小写不敏感，NULL类型只有一个取值，表示一个变量没有值，
+ 当被赋值为NULL
+ 未被赋值
+ 被unset()
这三种情况下变量被认为为NULL。

```php
<?php 
 $var;
 var_dump($var);
 $var1 = null;
 var_dump($var1);
 $var2 = NULL;
 var_dump($var2);
 $var3 = "节日快乐！";
 unset($var3);
 var_dump($var3);
?>

结果
NULL NULL NULL NULL
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
