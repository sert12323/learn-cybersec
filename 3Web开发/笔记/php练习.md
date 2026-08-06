[TOC]

# PHP变量

### PHP global 关键字

global全局变量无论在哪都能传参

```PHP
//输出结果等于 x+y=3
$z=1;
$y=2;
function add(){
    //全局变量z=全局变量x+全局变量y
    $GLOBALS['z']=$GLOBALS['x']+$GLOBALS['y'];
}
add();
echo $z;
```

### Static 作用域

Static 会 一直 保留参数数值

```PHP
<?php
function myTest()
{
    static $x=0;
    echo $x;
    $x++;
    echo PHP_EOL;    // 换行符
}
myTest();
myTest();
myTest();
?>
    //输出结果为 0 1 2 
```

# echo 和 print 语句

```php

$a = "1";
$b = "2";
$c= array("3", "4", "5");

echo $a;      //不加引号
echo "<br>";
echo "$b";     //加有引号
echo "<br>";
echo " {$c[0]}";

echo "<br>";

print "$a<br>$b<br>$c[0]";

echo "<br>";
print "$a";     //必须加引号
print "<br>";
print "$b";
print "<br>";
print " {$c[0]}";
```

# PHP EOF(heredoc)

- 必须后接分号，否则编译通不过。
- **EOF** 可以用任意其它字符代替，只需保证结束标识与开始标识一致。
- 结束标识必须顶格独自占一行(即必须从行首开始，前后不能衔接任何空白和字符)。
- 开始标识可以不带引号或带单双引号，不带引号与带双引号效果一致，解释内嵌的变量和转义符号，带单引号则不解释内嵌的变量和转义符号。
- 当内容需要内嵌引号（单引号或双引号）时，不需要加转义符，本身对单双引号转义，此处相当与q和qq的用法。

```
<?php
$name="runoob";
$a= <<<EOF
        "abc"$name
        "123"
EOF;
// 结束需要独立一行且前后不能空格
echo $a;
?>

//输出结果 "abc"runoob "123"
```

# PHP 数据类型

## PHP 整型

整数是一个没有小数的数字。

整数规则:

- 整数必须至少有一个数字 (0-9)

- 整数不能包含逗号或空格

- 整数是==没有小数点的==

- 整数可以是==正数或负数==

- 整型可以用三种格式来指定：==十进制， 十六进制（ 以 0x 为前缀）或八进制==（前缀为 0）。

  ` var_dump() `函数返回==变量的数据类型==和值

```
<?php 
$x = 5985;
var_dump($x);
echo "<br>"; 
$x = -345; // 负数 
var_dump($x);
echo "<br>"; 
$x = 0x8C; // 十六进制数
var_dump($x);
echo "<br>";
$x = 047; // 八进制数
var_dump($x);
?>
```

## PHP 浮点型

浮点数是==带小数部分==的数字，或是指数形式。

```
<?php 
$x = 10.365;
var_dump($x);
echo "<br>"; 
$x = 2.4e3;
var_dump($x);
echo "<br>"; 
$x = 8E-5;
var_dump($x);
?>
```

## PHP 布尔型

```
$x=true;
$y=false;
```

## PHP 数组

```php
<?php 
$cars=array("Volvo","BMW","Toyota");
var_dump($cars);
?>
```

## PHP 资源类型

使用 `get_resource_type()` 函数可以返回资源（resource）类型

```php
<?php
$c = mysql_connect();
echo get_resource_type($c)."\n";
// 打印：mysql link

$fp = fopen("foo","w");
echo get_resource_type($fp)."\n";
// 打印：file

$doc = new_xmldoc("1.0");
echo get_resource_type($doc->doc)."\n";
// 打印：domxml document
?>
```

# PHP 类型比较

- 松散比较：使用两个等号 **==** 比较，==只比较值==，不比较类型。
- 严格比较：用三个等号 **===** 比较，==除了比较值，也比较类型==。

```php
<?php
if(42 == "42") {
    echo '1、值相等';
}
 
echo PHP_EOL; // 换行符
 
if(42 === "42") {
    echo '2、类型相等';
} else {
    echo '3、类型不相等';
}
?>
```

### PHP中 比较 0、false、null

```php
0 == false: bool(true)
0 === false: bool(false)

0 == null: bool(true)
0 === null: bool(false)

false == null: bool(true)
false === null: bool(false)

"0" == false: bool(true)
"0" === false: bool(false)

"0" == null: bool(false)
"0" === null: bool(false)

"" == false: bool(true)
"" === false: bool(false)

"" == null: bool(true)
"" === null: bool(false)
```

# PHP 常量

PHP 中的常量是指一旦定义后其值==不能被改变==的标识符。

常量值被定义后，在脚本的其他==任何地方都不能被改变==。

常量可以用 **define()** 函数或 **const** 关键字来定义。

==常量在整个脚本中都可以使用==

1. **不变性**: 常量一旦定义，其值不能改变。
2. **全局作用域**: 常量在定义后，可以在整个脚本的任何地方使用，无需使用 `global` 关键字。
3. **数据类型**: 常量的值可以是标量数据类型（如布尔值、整数、浮点数、字符串）或数组（PHP 7 及以上版本）。
4. **区分大小写**: 常量名称默认是区分大小写的。如果需要定义大小写不敏感的常量，可以在 `define()` 函数的第三个参数设置为 `true`。

## 设置 PHP 常量

常量在定义后，默认是==全局变量==，可以在整个运行的脚本的==任何地方使用==。

```php
bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
```

该函数有三个参数:

- **name：**必选参数，常量名称，即标志符。

- **value：**必选参数，常量的值。

- **case_insensitive** ：可选参数，如果设置为 TRUE，该常量则大小写不敏感，默认是大小写敏感的。

  **注意：**==自 PHP 7.3.0 开始，定义不区分大小写的常量已被弃用。从 PHP 8.0.0 开始，只有 false 是可接受的值，==传递 true 将产生一个警告。

```php
<?php
// 区分大小写的常量名
define("GREETING", "欢迎访问 Runoob.com");
echo GREETING;    // 输出 "欢迎访问 Runoob.com"
echo '<br>';
echo greeting;   // 输出 "greeting"，但是有警告信息，表示该常量未定义
?>
```

### 使用 const 关键字

```php
const CONSTANT_NAME = "value";
```

```PHP
const SITE_URL = "https://www.runoob.com";
echo SITE_URL; // 输出 "https://www.runoob.com"
```

### 预定义常量

- `PHP_VERSION`：当前 PHP 解析器的版本。
- `PHP_OS`：服务器的操作系统。
- `PHP_INT_MAX`：最大的整数值。
- `E_ERROR`、`E_WARNING`、`E_PARSE` 等：错误报告级别。

```php
echo PHP_VERSION; // 输出 PHP 版本，例如 "7.4.1"
echo PHP_OS;      // 输出操作系统，例如 "Linux"
echo PHP_INT_MAX; // 输出最大的整数值，例如 "9223372036854775807"
```

### 常量数组（PHP 7 及以上版本）

在 PHP 7 及以上版本中，常量也可以是数组。

```php
define("FRUITS", [
    "Apple",
    "Banana",
    "Orange"
]);

echo FRUITS[0]; // 输出 "Apple"

const COLORS = [
    "Red",
    "Green",
    "Blue"
];

echo COLORS[1]; // 输出 "Green"
```

# PHP 字符串变量

## PHP 并置运算符

在 PHP 中，只有一个字符串运算符。

并置==运算符 `.`*用于把两个字符串值连接起来==

```php
<?php
$txt1="Hello world!";
$txt2="What a nice day!";
echo $txt1 . " " . $txt2;
?>
```

## PHP strlen() 函数

`strlen()` 函数返回==字符串的长度==（字节数）。

```php
<?php
echo strlen("Hello world!");
?> //返回长度
```

## PHP strpos() 函数

strpos() 函数用于在字符串内查找一个字符或一段指定的文本。

如果在字符串中找到匹配，该函数会返回第一个匹配的字符位置。如果未找到匹配，则返回 FALSE。

```php
<?php
echo strpos("Hello world!","world");
?>   //6
```

# PHP 运算符

```php
<?php 
$x=10; 
$y=6;
echo ($x + $y); // 输出16
echo '<br>';  // 换行
 
echo ($x - $y); // 输出4
echo '<br>';  // 换行
 
echo ($x * $y); // 输出60
echo '<br>';  // 换行
 
echo ($x / $y); // 输出1.6666666666667
echo '<br>';  // 换行
 
echo ($x % $y); // 输出4
echo '<br>';  // 换行
 
echo -$x;
?>
```

 **intdiv()**

```php
<?php
var_dump(intdiv(10, 3));
?>  int(3)
```

## PHP 递增/递减运算符

| 运算符 | 名称   | 描述                |
| :----- | :----- | :------------------ |
| ++ x   | 预递增 | x 加 1，然后返回 x  |
| x ++   | 后递增 | 返回 x，然后 x 加 1 |
| -- x   | 预递减 | x 减 1，然后返回 x  |
| x --   | 后递减 | 返回 x，然后 x 减 1 |

以下实例演示了使用递增/递减运算符得到的结果：

```
<?php $x=10;  echo ++$x; // 输出11  $y=10;  echo $y++; // 输出10  $z=5; echo --$z; // 输出4  $i=5; echo $i--; // 输出5 ?>
```

## PHP 逻辑运算符

| 运算符   | 名称 | 描述                                         |                 实例                 |
| :------- | :--- | :------------------------------------------- | :----------------------------------: |
| x and y  | 与   | 如果 x 和 y 都为 true，则返回 true           | x=6 y=3 (x < 10 and y > 1) 返回 true |
| x or y   | 或   | 如果 x 和 y 至少有一个为 true，则返回 true   |   x=6 y=3 (x==6 or y==5) 返回 true   |
| x xor y  | 异或 | 如果 x 和 y 有且仅有一个为 true，则返回 true |  x=6 y=3 (x==6 xor y==3) 返回 false  |
| x && y   | 与   | 如果 x 和 y 都为 true，则返回 true           | x=6 y=3 (x < 10 && y > 1) 返回 true  |
| x \|\| y | 或   | 如果 x 和 y 至少有一个为 true，则返回 true   | x=6 y=3 (x==5 \|\| y==5) 返回 false  |
| ! x      | 非   | 如果 x 不为 true，则返回 true                |      x=6 y=3 !(x==y) 返回 true       |

## 三元运算符

### 语法格式

```
(expr1) ? (expr2) : (expr3) 
```

对 ==expr1 求值为 TRUE 时的值为 expr2==，==在 expr1 求值为 FALSE 时的值为 expr3==。

自 PHP 5.3 起，可以省略三元运算符中间那部分。表达式 expr1 ?: expr3 在 expr1 求值为 TRUE 时返回 expr1，否则返回 expr3。

```php
<?php
// 普通写法
$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';
echo $username, PHP_EOL;
 
// PHP 5.3+ 版本写法
$username = $_GET['user'] ?: 'nobody';
echo $username, PHP_EOL;
?>
```

在 PHP7+ 版本多了一个 NULL 合并运算符 **??**

```php
<?php
// 如果 $_GET['user'] 不存在返回 'nobody'，否则返回 $_GET['user'] 的值
$username = $_GET['user'] ?? 'nobody';
// 类似的三元运算符
$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';
?>
```

## 组合比较符(PHP7+)

```
$c = $a <=> $b;
```

解析如下：

- 如果 **$a > $b**, 则 **$c** 的值为 **1**。
- 如果 **$a == $b**, 则 **$c** 的值为 **0**。
- 如果 **$a < $b**, 则 **$c** 的值为 **-1**。

```php
<?php
// 整型
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1
 
// 浮点型
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1
 
// 字符串
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1
?>
```

## 运算符优先级

```php
<?php
// 优先级： &&  >  =  >  and
// 优先级： ||  >  =  >  or
 
$a = 3;
$b = false;
$c = $a or $b;
var_dump($c);          // 这里的 $c 为 int 值3，而不是 boolean 值 true
$d = $a || $b;
var_dump($d);          //这里的 $d 就是 boolean 值 true 
?>
```

# PHP If...Else 语句

## PHP - if...else 语句

**在条件成立时执行一块代码，条件不成立时执行另一块代码**，请使用 if....else 语句。

### 语法

```
if (条件) {
    条件成立时执行的代码;
} else {
    条件不成立时执行的代码;
}
```

如果当前时间小于 20，下面的实例将输出 "Have a good day!"，否则输出 "Have a good night!"：

```php
<?php $t=date("H"); if ($t<"20")
{    echo "Have a good day!"; } 
else 
{    echo "Have a good night!"; } ?>
```

# PHP Switch 语句

- `expression` 是要被比较的表达式。
- `case value:` 是可能的值，如果 `expression` 的值等于某个 `case` 的值，就执行相应的代码块。
- `break;` 用于终止 `switch` 语句，防止继续执行下一个 `case`。
- `default:` 是可选的，用于指定当没有匹配的 `case` 时执行的代码块。

```php
<?php
$favcolor="red";
switch ($favcolor)
{
case "red":
    echo "你喜欢的颜色是红色!";
    break;
case "blue":
    echo "你喜欢的颜色是蓝色!";
    break;
case "green":
    echo "你喜欢的颜色是绿色!";
    break;
default:
    echo "你喜欢的颜色不是 红, 蓝, 或绿色!";
}
?>
```

# PHP 数组

`array();`

```php
<?php
$cars=array("Volvo","BMW","Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
?>
```

## 获取数组的长度 - count() 函数

count() 函数用于返回数组的长度（元素的数量）：

```php
<?php
$cars=array("Volvo","BMW","Toyota");
echo count($cars);
?>
```

## 遍历数值数组

```php
<?php
$cars=array("Volvo","BMW","Toyota");
$arrlength=count($cars);
 
for($x=0;$x<$arrlength;$x++)
{
    echo $cars[$x];
    echo "<br>";
}
?>
```

## PHP 关联数组

关联数组是使用您分配给数组的指定的键的数组。

这里有两种创建关联数组的方法：

```php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");

or:

$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43";
```

```php
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
echo "Peter is " . $age['Peter'] . " years old.";
?>
```

# PHP 数组排序

## PHP - 数组排序函数

在本章中，我们将一一介绍下列 PHP 数组排序函数：

- sort() - 对数组进行==升序==排列
- rsort() - 对数组进行==降序==排列
- asort() - 根据关联数组的==值==，对数组进行升序排列
- ksort() - 根据关联数组的==键==，对数组进行升序排列
- arsort() - 根据关联数组的==值==，对数组进行降序排列
- krsort() - 根据关联数组的==键==，对数组进行降序排列

## sort() - 对数组进行升序排列

升序

```php
<?php
	$cars=array("Volvo","BMW","Toyota");sort($cars);?>
```

```php
<?php
	$numbers=array(4,6,2,22,11);sort($numbers);?>
```

## rsort() - 对数组进行降序排列

略

# PHP 超级全局变量

## PHP 超级全局变量

PHP中预定义了几个超级全局变量（superglobals） ，这意味着它们在一个脚本的全部作用域中都可用。 你不需要特别说明，就可以在函数及类中使用。

PHP 超级全局变量列表:

- $GLOBALS

- $_SERVER

- $_REQUEST

- $_POST

- $_GET

- $_FILES

- $_ENV

- $_COOKIE

  ### PHP $_SERVER

  $_SESSION$_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建。不能保证每个服务器都提供全部项目；服务器可能会忽略一些，或者提供一些没有在这里列举出来的项目。

  以下实例中展示了如何使用$_SERVER中的元素:

| 元素/代码                       | 描述                                                         |
| :------------------------------ | :----------------------------------------------------------- |
| $_SERVER['PHP_SELF']            | 当前执行脚本的文件名，与 document root 有关。例如，在地址为 http://example.com/test.php/foo.bar 的脚本中使用 $_SERVER['PHP_SELF'] 将得到 /test.php/foo.bar。__FILE__ 常量包含当前(例如包含)文件的完整路径和文件名。 从 PHP 4.3.0 版本开始，如果 PHP 以命令行模式运行，这个变量将包含脚本名。之前的版本该变量不可用。 |
| $_SERVER['GATEWAY_INTERFACE']   | 服务器使用的 CGI 规范的版本；例如，"CGI/1.1"。               |
| $_SERVER['SERVER_ADDR']         | 当前运行脚本所在的服务器的 IP 地址。                         |
| $_SERVER['SERVER_NAME']         | 当前运行脚本所在的服务器的主机名。如果脚本运行于虚拟主机中，该名称是由那个虚拟主机所设置的值决定。(如: www.runoob.com) |
| $_SERVER['SERVER_SOFTWARE']     | 服务器标识字符串，在响应请求时的头信息中给出。 (如：Apache/2.2.24) |
| $_SERVER['SERVER_PROTOCOL']     | 请求页面时通信协议的名称和版本。例如，"HTTP/1.0"。           |
| $_SERVER['REQUEST_METHOD']      | 访问页面使用的请求方法；例如，"GET", "HEAD"，"POST"，"PUT"。 |
| $_SERVER['REQUEST_TIME']        | 请求开始时的时间戳。从 PHP 5.1.0 起可用。 (如：1377687496)   |
| $_SERVER['QUERY_STRING']        | query string（查询字符串），如果有的话，通过它进行页面访问。 |
| $_SERVER['HTTP_ACCEPT']         | 当前请求头中 Accept: 项的内容，如果存在的话。                |
| $_SERVER['HTTP_ACCEPT_CHARSET'] | 当前请求头中 Accept-Charset: 项的内容，如果存在的话。例如："iso-8859-1,*,utf-8"。 |
| $_SERVER['HTTP_HOST']           | 当前请求头中 Host: 项的内容，如果存在的话。                  |
| $_SERVER['HTTP_REFERER']        | 引导用户代理到当前页的前一页的地址（如果存在）。由 user agent 设置决定。并不是所有的用户代理都会设置该项，有的还提供了修改 HTTP_REFERER 的功能。简言之，该值并不可信。) |
| $_SERVER['HTTPS']               | 如果脚本是通过 HTTPS 协议被访问，则被设为一个非空的值。      |
| $_SERVER['REMOTE_ADDR']         | 浏览当前页面的用户的 IP 地址。                               |
| $_SERVER['REMOTE_HOST']         | 浏览当前页面的用户的主机名。DNS 反向解析不依赖于用户的 REMOTE_ADDR。 |
| $_SERVER['REMOTE_PORT']         | 用户机器上连接到 Web 服务器所使用的端口号。                  |
| $_SERVER['SCRIPT_FILENAME']     | 当前执行脚本的绝对路径。                                     |
| $_SERVER['SERVER_ADMIN']        | 该值指明了 Apache 服务器配置文件中的 SERVER_ADMIN 参数。如果脚本运行在一个虚拟主机上，则该值是那个虚拟主机的值。(如：someone@runoob.com) |
| $_SERVER['SERVER_PORT']         | Web 服务器使用的端口。默认值为 "80"。如果使用 SSL 安全连接，则这个值为用户设置的 HTTP 端口。 |
| $_SERVER['SERVER_SIGNATURE']    | 包含了服务器版本和虚拟主机名的字符串。                       |
| $_SERVER['PATH_TRANSLATED']     | 当前脚本所在文件系统（非文档根目录）的基本路径。这是在服务器进行虚拟到真实路径的映像后的结果。 |
| $_SERVER['SCRIPT_NAME']         | 包含当前脚本的路径。这在页面需要指向自己时非常有用。__FILE__ 常量包含当前脚本(例如包含文件)的完整路径和文件名。 |
| $_SERVER['SCRIPT_URI']          | URI 用来指定要访问的页面。例如 "/index.html"。               |

## PHP $_REQUEST