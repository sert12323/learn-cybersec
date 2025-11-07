# 22安全开发-PHP应用&留言板功能&超全局变量&数据库操作&第三方插件引用

<img src="C:\Users\Yennefer\AppData\Roaming\Typora\typora-user-images\image-20251104144025216.png" alt="image-20251104144025216" style="zoom: 80%;" />

\#开发环境：

DW + PHPStorm + PhpStudy + Navicat Premium

DW : HTML&JS&CSS开发

PHPStorm : 专业PHP开发IDE

PhpStudy ：Apache MYSQL环境

Navicat Premium: 全能数据库管理工具

 

\#数据导入-mysql架构&库表列

1、数据库名，数据库表名，数据库列名

2、数据库数据，格式类型，长度，键等

 

\#数据库操作-mysqli函数&增删改查

PHP函数：连接，选择，执行，结果，关闭等

参考：https://www.runoob.com/php/php-ref-mysqli.html

常用：

mysqli_connect() 打开一个到MySQL的新的连接。

mysqli_select_db() 更改连接的默认数据库。

mysqli_query() 执行某个针对数据库的查询。

mysqli_fetch_row() 从结果集中取得一行，并作为枚举数组返回。

mysqli_close() 关闭先前打开的数据库连接。

## :dagger:MYSQL增删改查：

查：select * from 表名 where 列名='条件';

增：insert into 表名(`列名1`, `列名2`) value('列1值1', '列2值2');

删：delete from 表名 where 列名 = '条件';

改：update 表名 set 列名 = 数据 where 列名 = '条件';

 

\#数据接收输出-html混编&超全局变量

1、html混编：使HTML(JS)在PHP语言中运行

<?php

echo '<script>alert('x');</script>'

?>

2、超全局变量：

参考：

https://www.w3school.com.cn/php/php_superglobals.asp

https://www.php.net/manual/zh/language.variables.superglobals.php

$GLOBALS：这种全局变量用于在 PHP 脚本中的任意位置访问全局变量

$_SERVER：这种超全局变量保存关于报头、路径和脚本位置的信息。

$_REQUEST：$_REQUEST 用于收集 HTML 表单提交的数据。

$_POST：广泛用于收集提交method="post" 的HTML表单后的表单数据。

$_GET：收集URL中的发送的数据。也可用于收集提交HTML表单数据(method="get") $_FILES：文件上传且处理包含通过HTTP POST方法上传给当前脚本的文件内容。

$_ENV：是一个包含服务器端环境变量的数组。

$_COOKIE：是一个关联数组，包含通过cookie传递给当前脚本的内容。

$_SESSION：是一个关联数组，包含当前脚本中的所有session内容。

 

\#第三方插件引用-js传参&函数对象调用

引用：<script src='../xxx.js'></script>

函数对象调用：

var obj = {

  value : 0,

  increment : function (inc) {  

​    this.value += typeof inc === 'number' ? inc :1;

​    //设置inc且为数字时 value=inc 反之 value=1

  }

}

obj.increment();

console.log(obj.value);  //1

obj.increment(2);

console.log(obj.value);  //2