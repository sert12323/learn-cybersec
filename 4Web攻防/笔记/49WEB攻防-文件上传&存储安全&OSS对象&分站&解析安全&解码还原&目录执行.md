# WEB攻防-文件上传&存储安全&OSS对象&分站&解析安全&解码还原&目录执行

\#文件-解析方案-执行权限&解码还原

1、执行权限

文件上传后存储目录不给执行权限

2、解码还原

数据做存储，解析固定（文件后缀名无关）

文件上传后利用编码传输解码还原

 

\#文件-存储方案-分站存储&OSS对象

1、分站存储

upload.xiaodi8.com 上传

images.xiaodi8.com 存储

2、OSS对象

Access控制-OSS对象存储-Bucket对象

 

\#如何判断

实例分析判断

 

\#安全绕过

以上方案除目录设置权限如能换目录解析绕过外，其他均无解

\#文件安全-读取下载&删除-黑白盒

1、下载=读取

常规下载URL：http://www.xiaodi8.com/upload/123.pdf

可能存在安全URL：http://www.xiaodi8.com/xx.xx?file=123.pdf

利用：常规下载敏感文件（数据库配置，中间件配置，系统密匙等文件信息）

2、文件删除（常出现后台中）

可能存在安全问题：前台或后台有删除功能应用

利用：常规删除重装锁定配合程序重装或高危操作

 

\#目录安全-遍历&穿越-黑白盒

1、目录索引

目录权限控制不当，通过遍历获取到有价值的信息文件去利用

2、目录穿越（路径遍历常出现后台中）

目录权限控制不当，通过控制查看目录路径穿越到其他目录或判断获取价值文件再利用

 

\#黑盒分析：

1、功能点

文件上传，文件下载，文件删除，文件管理器等地方

2、URL特征

文件名：

download，down，readfile，read，del，dir，path，src，Lang等

参数名：

file、path、data、filepath、readfile、data、url、realpath等

\#白盒分析：

上传类，删除类，下载类，目录操作函数，读取查看方法或函数等

 

\#复盘

https://mp.weixin.qq.com/s/Kiec7FhvpAmPf7zTWvJa3g

https://mp.weixin.qq.com/s/QqXxpTwXSjCNN622fSsZAQ

https://mp.weixin.qq.com/s/A2FvZMuPpHiewGrL5UDlHw

https://mp.weixin.qq.com/s/w2PH7EI6Z2R9diV_tBA1Zw