# 25安全开发-PHP应用&文件管理模块&包含&上传&遍历&写入&删除&下载&安全

\#PHP文件操作安全

-文件包含，文件上传，文件下载，文件删除，文件写入，文件遍历

 

\#文件包含：

include() 在错误发生后脚本继续执行

require() 在错误发生后脚本停止执行

include_once() 如果已经包含，则不再执行

require_once() 如果已经包含，则不再执行

 

\#文件上传：

1、无过滤机制

2、黑名单过滤机制

3、白名单过滤机制

4、文件类型过滤机制

 

架构：

1、上传至服务器本身的存储磁盘(源码在一起)

2、云产品OSS存储对象去存储文件(泄漏安全)

3、把文件上传到其他域名,如：www.xiaodi8.com->upload.xiaodi8.com

 

$_FILES：PHP中一个预定义的超全局变量，用于在上传文件时从客户端接收文件，并将其保存到服务器上。它是一个包含上传文件信息的数组，包括文件名、类型、大小、临时文件名等信息。

$_FILES["表单值"]["name"] 获取上传文件原始名称

$_FILES["表单值"]["type"] 获取上传文件MIME类型

$_FILES["表单值"]["size"] 获取上传文件字节单位大小

$_FILES["表单值"]["tmp_name"] 获取上传的临时副本文件名

$_FILES["表单值"]["error"] 获取上传时发生的错误代码

move_uploaded_file() 将上传的文件移动到指定位置的函数

 

\#文件显示：

1.打开目录读取文件列表

2.递归循环读取文件列表

3.判断是文件还是文件夹

4.PHP.INI目录访问控制

is_dir() 函数用于检查指定的路径是否是一个目录

opendir() 函数用于打开指定的目录，返回句柄，用来读取目录中的文件和子目录

readdir() 函数用于从打开的目录句柄中读取目录中的文件和子目录

open_basedir：PHP.INI中的设置用来控制脚本程序访问目录

ini_set('open_basedir',__DIR__); 设置配置文件中，只能访问本目录

 

\#文件删除：

unlink() 文件删除函数

调用命令删除：system shell_exec exec等

 

\#文件下载：

修改HTTP头实现文件读取解析下载：

header("Content-Type: application/octet-stream");

header("Content-Disposition: attachment; filename="");

header("Content-Length: " . filesize($file));

readfile($file);

 

\#文件编辑：

1、file_get_contents() 读取文件内容

2、fopen() fread() 文件打开读入