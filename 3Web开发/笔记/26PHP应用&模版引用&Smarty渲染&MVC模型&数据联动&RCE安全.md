# PHP应用&模版引用&Smarty渲染&MVC模型&数据联动&RCE安全

\#新闻列表

1、数据库创建新闻存储

2、代码连接数据库读取

3、页面进行自定义显示

 

\#自写模版引用

1、页面显示样式编排

2、显示数据插入页面

3、引用模版调用触发

 

\#Smarty模版引用

下载：https://github.com/smarty-php/smarty/releases

使用：

1、创建一个文件夹，命名为smarty-demo。

2、下载Smarty对应版本并解压缩到该文件夹中。

3、创建一个PHP文件，命名为index.php，并在文件中添加以下代码：

<?php

// 引入 Smarty 类文件

require('smarty-demo/libs/Smarty.class.php');

// 创建 Smarty 实例

$smarty = new Smarty;

// 设置 Smarty 相关属性

$smarty->template_dir = 'smarty-demo/templates/';

$smarty->compile_dir = 'smarty-demo/templates_c/';

$smarty->cache_dir = 'smarty-demo/cache/';

$smarty->config_dir = 'smarty-demo/configs/';

// 赋值变量到模板中

$smarty->assign('title', '欢迎使用 Smarty');

// 显示模板

$smarty->display('index.tpl');

?>

4、创建一个名为index.tpl的模板文件，并将以下代码复制到上述点定义文件夹中

<!DOCTYPE html>

<html>

<head>

<title>{$title}</title>

</head>

<body>

<h1>{$title}</h1>

<p>这是一个使用 Smarty 的例子。</p>

</body>

</html>

 

\#代码RCE安全测试

1、自写模版的安全隐患

2、第三方Smarty的安全隐患