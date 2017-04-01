---
title: 会话控制（session与cookie）
date: 2016-10-17 11:19:03
categories: php
---
会话控制（session与cookie）
<!-- more -->

<h2>cookie</h2>
Cookie是存储在客户端浏览器中的数据，我们通过Cookie来跟踪与存储用户数据。一般情况下，Cookie通过HTTP headers从服务端返回到客户端。多数web程序都支持Cookie的操作，因为Cookie是存在于HTTP的标头之中，所以必须在其他信息输出以前进行设置，类似于header函数的使用限制。
PHP通过setcookie函数进行Cookie的设置，任何从浏览器发回的Cookie，PHP都会自动的将他存储在$_COOKIE的全局变量之中，因此我们可以通过$_COOKIE['key']的形式来读取某个Cookie值。
PHP中的Cookie具有非常广泛的使用，经常用来存储用户的登录信息，购物车等，且在使用会话Session时通常使用Cookie来存储会话id来识别用户，Cookie具备有效期，当有效期结束之后，Cookie会自动的从客户端删除。同时为了进行安全控制，Cookie还可以设置域跟路径，我们会在稍后的章节中详细的讲解他们。

<h3>设置Cookie</h3>
PHP设置Cookie最常用的方法就是使用setcookie函数，setcookie具有7个可选参数，我们常用到的为前5个：
+ name（ Cookie名）可以通过$_COOKIE['name'] 进行访问
+ value（Cookie的值）
+ expire（过期时间）Unix时间戳格式，默认为0，表示浏览器关闭即失效
+ path（有效路径）如果路径设置为'/'，则整个网站都有效
+ domain（有效域）默认整个域名都有效，如果设置了'www.imooc.com',则只在www子域中有效

```php
$value = 'test';
setcookie("TestCookie", $value);
setcookie("TestCookie", $value, time()+3600);  //有效期一小时
setcookie("TestCookie", $value, time()+3600, "/path/", "imooc.com"); //设置路径与域
```
PHP中还有一个设置Cookie的函数setrawcookie，setrawcookie跟setcookie基本一样，唯一的不同就是value值不会自动的进行urlencode，因此在需要的时候要手动的进行urlencode。

```php
setrawcookie('cookie_name', rawurlencode($value), time()+60*60*24*365); 
//因为Cookie是通过HTTP标头进行设置的，所以也可以直接使用header方法进行设置。
header("Set-Cookie:cookie_name=value");
```
<h3>cookie的删除与过期时间</h3>
setcookie('test', '', time()-1); 
可以看到将cookie的过期时间设置到当前时间之前，则该cookie会自动失效，也就达到了删除cookie的目的。之所以这么设计是因为cookie是通过HTTP的标头来传递的，客户端根据服务端返回的Set-Cookie段来进行cookie的设置，如果删除cookie需要使用新的Del-Cookie来实现，则HTTP头就会变得复杂，实际上仅通过Set-Cookie就可以简单明了的实现Cookie的设置、更新与删除。
了解原理以后，我们也可以直接通过header来删除cookie。

```php
header("Set-Cookie:test=1393832059; expires=".gmdate('D, d M Y H:i:s \G\M\T', time()-1));
```
这里用到了gmdate，用来生成格林威治标准时间，以便排除时差的影响。

<h3>cookie的有效路径</h3>
cookie中的路径用来控制设置的cookie在哪个路径下有效，默认为'/'，在所有路径下都有，当设定了其他路径之后，则只在设定的路径以及子路径下有效，例如：
setcookie('test', time(), 0, '/path');
上面的设置会使test在/path以及子路径/path/abc下都有效，但是在根目录下就读取不到test的cookie值。
一般情况下，大多是使用所有路径的，只有在极少数有特殊需求的时候，会设置路径，这种情况下只在指定的路径中才会传递cookie值，可以节省数据的传输，增强安全性以及提高性能。
当我们设置了有效路径的时候，不在当前路径的时候则看不到当前cookie。

```php
setcookie('test', '1',0, '/path');  
var_dump($_COOKIE['test']);  
```
<h3>session</h3>
<h3>session与cookie的异同</h3>
cookie将数据存储在客户端，建立起用户与服务器之间的联系，通常可以解决很多问题，但是cookie仍然具有一些局限：
cookie相对不是太安全，容易被盗用导致cookie欺骗
<font color=#FF6666>单个cookie的值最大只能存储4k</font>
每次请求都要进行网络传输，占用带宽
session是将用户的会话数据存储在服务端，没有大小限制，通过一个session_id进行用户识别，PHP默认情况下session id是通过cookie来保存的，因此从某种程度上来说，seesion依赖于cookie。但这不是绝对的，session id也可以通过参数来实现，只要能将session id传递到服务端进行识别的机制都可以使用session。

```php
<?php
//开始使用session
session_start();
//设置一个session
$_SESSION['test'] = time();
//显示当前的session_id
echo "session_id:".session_id();
echo "<br>";

//读取session值
echo $_SESSION['test'];

//销毁一个session
unset($_SESSION['test']);
echo "<br>";
var_dump($_SESSION);
```

<h3>使用session</h3>
在PHP中使用session非常简单，先执行session_start方法开启session，然后通过全局变量$_SESSION进行session的读写。

```php
session_start();
$_SESSION['test'] = time();
var_dump($_SESSION);
```

session会自动的对要设置的值进行encode与decode，因此session可以支持任意数据类型，包括数据与对象等。

```php
session_start();
$_SESSION['ary'] = array('name' => 'jobs');
$_SESSION['obj'] = new stdClass();
var_dump($_SESSION);
```
默认情况下，session是以文件形式存储在服务器上的，因此当一个页面开启了session之后，会独占这个session文件，这样会导致当前用户的其他并发访问无法执行而等待。可以采用缓存或者数据库的形式存储来解决这个问题，

<h3>删除与销毁session</h3>
删除某个session值可以使用PHP的unset函数，删除后就会从全局变量$_SESSION中去除，无法访问。

```php
session_start();
$_SESSION['name'] = 'jobs';
unset($_SESSION['name']);
echo $_SESSION['name']; //提示name不存在
```
如果要删除所有的session，可以使用session_destroy函数销毁当前session，session_destroy会删除所有数据，但是session_id仍然存在。

```php
session_start();
$_SESSION['name'] = 'jobs';
$_SESSION['time'] = time();
session_destroy();
```
值得注意的是，session_destroy并不会立即的销毁全局变量$_SESSION中的值，只有当下次再访问的时候，$_SESSION才为空，因此如果需要立即销毁$_SESSION，可以使用unset函数。

```php
session_start();
$_SESSION['name'] = 'jobs';
$_SESSION['time'] = time();
unset($_SESSION);
session_destroy(); 
var_dump($_SESSION); //此时已为空
```
如果需要同时销毁cookie中的session_id，通常在用户退出的时候可能会用到，则还需要显式的调用setcookie方法删除session_id的cookie值。




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
