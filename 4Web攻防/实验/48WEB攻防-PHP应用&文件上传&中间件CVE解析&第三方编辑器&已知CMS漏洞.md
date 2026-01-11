# WEB攻防-PHP应用&文件上传&中间件CVE解析&第三方编辑器&已知CMS漏洞

\#PHP-中间件-上传相关-Apache&Nginx

复现漏洞环境：vulhub （部署搭建看打包视频）

由于PHP搭建常用中间件：IIS，Apache，Nginx

Web搭建在存在漏洞的中间件上，漏洞影响这文件的解析即配合上传

 

\#PHP-编辑器-上传相关-第三方处理引用

复现漏洞环境：ueditor （部署搭建看打包视频）

由于编辑器漏洞较少，实战碰到机会不大，主要理解漏洞产生的思路

参考：https://cloud.tencent.com/developer/article/2200036

参考：https://blog.csdn.net/qq_45813980/article/details/126866682

引用到外部的第三方编辑器实现文件上传，编辑器的安全即是上传安全

 

\#PHP-CMS源码-上传相关-已知识别到利用

复现漏洞环境：通达OA-V11.2

从未知的源码体系测试原生态上传安全，现在是已知CMS源码架构，利用已知的漏洞测试