# JS应用&NodeJS指南&原型链污染&Express框架&功能实现&审计

\#环境搭建-NodeJS-解析安装&库安装

0、文档参考：

https://www.w3cschool.cn/nodejs/

1、Nodejs安装

https://nodejs.org/en

 

2、三方库安装

express

Express是一个简洁而灵活的node.js Web应用框架

 

body-parser

node.js中间件，用于处理 JSON, Raw, Text和URL编码的数据。

 

cookie-parser

这就是一个解析Cookie的工具。通过req.cookies可以取到传过来的cookie，并把它们转成对象。

 

multer

node.js中间件，用于处理 enctype="multipart/form-data"（设置表单的MIME编码）的表单数据。

 

mysql

Node.js来连接MySQL专用库，并对数据库进行操作。

 

安装命令：

npm i express

npm i body-parser

npm i cookie-parser

npm i multer

npm i mysql

 

\#功能实现-NodeJS-数据库&文件&执行

an

1、Express开发 

2、实现用户登录 

3、加入数据库操作

 

-文件操作

1、Express开发

2、实现目录读取

3、加入传参接受

 

-命令执行（RCE）

1、eval

2、exec & spawnSync

 

\#安全问题-NodeJS-注入&RCE&原型链

1、SQL注入&文件操作

2、RCE执行&原型链污染

2、NodeJS黑盒无代码分析

实战测试NodeJS安全：

判断：参考前期的信息收集

黑盒：通过对各种功能和参数进行payload测试

白盒：通过对代码中写法安全进行审计分析

-原型链污染

如果攻击者控制并修改了一个对象的原型，(__proto__)

那么将可以影响所有和这个对象来自同一个类、父祖类的对象。

 

\#案例分析-NodeJS-CTF题目&源码审计

1、CTFSHOW几个题目

https://ctf.show/ Web334-344

https://f1veseven.github.io/2022/04/03/ctf-nodejs-zhi-yi-xie-xiao-zhi-shi/

2、YApi管理平台漏洞

https://blog.csdn.net/weixin_42353842/article/details/127960229

 

\#开发指南-NodeJS-安全SecGuide项目

https://github.com/Tencent/secguide