# 13信息打点-JS架构&框架识别&泄漏提取&API接口枚举&FUZZ爬虫&插件项

\#知识点：

1、业务资产-应用类型分类

2、Web单域名获取-接口查询

3、Web子域名获取-解析枚举

4、Web架构资产-平台指纹识别

\------------------------------------

1、开源-CMS指纹识别源码获取方式

2、闭源-习惯&配置&特性等获取方式

3、闭源-托管资产平台资源搜索监控

\------------------------------------

1、JS前端架构-识别&分析

2、JS前端架构-开发框架分析

3、JS前端架构-打包器分析

4、JS前端架构-提取&FUZZ

解决：

1、如何从表现中的JS提取价值信息

2、如何从地址中FUZZ提取未知的JS文件

3、如何从JS开放框架WebPack进行测试

 

\#章节点

Web：语言/CMS/中间件/数据库/系统/WAF等

系统：操作系统/端口服务/网络环境/防火墙等

应用：APP对象/API接口/微信小程序/PC应用等

架构：CDN/前后端/云应用/站库分离/OSS资源等

技术：JS爬虫/敏感扫描/端口扫描/源码获取/接口泄漏等

技术：指纹识别/Github监控/CDN绕过/WAF识别/蜜罐识别等

 

\#补充：

CMS

Discuz、WordPress、Ecshop、蝉知等

 

前端技术

HTML5、jquery、bootstrap、Vue等

 

开发语言

PHP、JAVA、Ruby、Python、C#，JS等

 

Web服务器

Apache、Nginx、IIS、lighttpd，Apache等

 

应用服务器：

Tomcat、Jboss、Weblogic、Websphere等

 

数据库类型：

Mysql、SqlServer、Oracle、Redis、MongoDB等

 

操作系统信息

Linux、windows等

 

应用服务信息：

FTP、SSH、RDP、SMB、SMTP、LDAP、Rsync等

 

CDN信息

帝联、Cloudflare、网宿、七牛云、阿里云等

 

WAF信息

创宇盾、宝塔、ModSecurity、玄武盾、OpenRASP等。

 

蜜罐信息：

HFish、TeaPot、T-Pot、Glastopf等

 

其他组件信息

fastjson、shiro、log4j、OA办公等

0、什么是JS渗透测试？

  在Javascript中也存在变量和函数，当存在可控变量及函数调用即可参数漏洞

JS开发的WEB应用和PHP，JAVA,NET等区别在于即没有源代码，也可以通过浏览器的查看源代码获取真实的点。获取URL，获取JS敏感信息，获取代码传参等，所以相当于JS开发的WEB应用属于白盒测试（默认有源码参考），一般会在JS中寻找更多的URL地址，在JS代码逻辑（加密算法，APIkey配置，验证逻辑等）进行后期安全测试。

 

前提：Web应用可以采用后端或前端语言开发

-后端语言：php java python .NET 浏览器端看不到真实的源代码

-前端语言：JavaScript(JS)和JS框架 浏览器端看到真实的源代码

例子：

zblog：核心功能采用PHP语言去传输接受

vue.js：核心功能采用框架语法（JS）传输接受

 

1、JS安全问题

  源码泄漏

  未授权访问=JS里面分析更多的URL访问确定接口路径

  敏感key泄漏=JS文件中可能配置了接口信息（云应用，短信，邮件，数据库等）

  API接口安全=（代码中加密提交参数传递，更多的URL路径）

 

2、流行的Js框架有那些？

  Vue NodeJS jQuery Angular等

 

3、如何判定JS开发应用？

  插件wappalyzer

  源程序代码简短

  引入多个js文件

  一般有/static/js/app.js等顺序的js文件

  一般cookie中有connect.sid

4、如何获取更多的JS文件？

  手工-浏览器搜索

  半自动-Burpsuite插件

  工具化-各类提取&FUZZ项目

5、如何快速获取价值信息？

  src=

  path=

  method:"get"

  http.get("

  method:"post"

  http.post("

  $.ajax

  http://service.httppost

  http://service.httpget

 

\#前端架构-手工搜索分析

浏览器全局搜索分析

 

\#前端架构-半自动Burp分析

自带功能：Target->sitemap->Engagement tools->Find scripts

官方插件：JS Link Finder & JS Miner

第三方插件：HaE & Unexpected_information

插件加载器：jython-standalone-2.7.2

 

Unexpected_information：https://github.com/ScriptKid-Beta/Unexpected_information

用来标记请求包中的一些敏感信息、JS接口和一些特殊字段，

防止我们疏忽了一些数据包，使用它可能会有意外的收获信息。

HaE：

https://github.com/gh0stkey/HaE

https://raw.githubusercontent.com/gh0stkey/HaE/gh-pages/Config.yml

基于BurpSuite插件JavaAPI开发的请求高亮标记与信息提取的辅助型插件。该插件可以通过自定义正则的方式匹配响应报文或请求报文，可以自行决定符合该自定义正则匹配的相应请求是否需要高亮标记、信息提取。

 

\#前端架构-自动化项目分析

Jsfinder-从表现中JS中提取URL或者敏感数据

https://github.com/Threezh1/JSFinder

一款用作快速在网站的js文件中提取URL，子域名的工具

 

URLFinder-从表现中JS中提取URL或者敏感数据

https://github.com/pingc0y/URLFinder

一款用于快速提取检测页面中JS与URL的工具。

功能类似于JSFinder，但JSFinder好久没更新了。

 

JSINFO-SCAN-从表现中JS中提取URL或者敏感数据

https://github.com/p1g3/JSINFO-SCAN

递归爬取域名(netloc/domain)，以及递归从JS中获取信息的工具

 

FindSomething-从表现中JS中提取URL或者敏感数据

https://github.com/momosecurity/FindSomething

该工具是用于快速在网页的html源码或js代码中提取一些有趣的信息的浏览器插件，

包括请求的资源、接口的url，请求的ip和域名，泄漏的证件号、手机号、邮箱等信息。

 

ffuf-FUZZ爆破找到更多的js文件分析更多的信息

https://github.com/ffuf/ffuf

https://wordlists.assetnote.io

功能强大的模糊化工具，用它来FUZZ模糊化js文件。

 

Packer-Fuzzer-针对JS框架开发打包器Webpack检测

https://github.com/rtcatc/Packer-Fuzzer

一款针对Webpack等前端打包工具所构造的网站进行快速、高效安全检测的扫描工具