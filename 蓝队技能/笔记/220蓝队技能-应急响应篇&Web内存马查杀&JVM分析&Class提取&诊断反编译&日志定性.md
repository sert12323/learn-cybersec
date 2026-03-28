# 蓝队技能-应急响应篇&Web内存马查杀&JVM分析&Class提取&诊断反编译&日志定性

```
#Java内存马查杀
0、环境搭建
安装tomcat
安装jdk
配置setclasspath.bat
启动startup.bat
tomcat,jdk安装包见资源网盘
https://blog.csdn.net/weixin_45910254/article/details/129694499

1、查杀脚本
项目地址：https://github.com/c0ny1/java-memshell-scanner
通过jsp脚本扫描并查杀各类中间件内存马，比Java agent要温和一些。

2、查杀项目
项目地址：https://github.com/alibaba/arthas
arthas为一款监控诊断产品，通过全局视角实时查看应用load、内存、gc、线程的状态信息。可使用该工具对内存马排查和分析，如攻击者隐藏的深可将所有的类都反编译导出来然后逐一排查。

查看URL路由（看Servlet内存马）
mbean | grep "name=/"

sc查看JVM 已加载的类信息
sc *.Filter
sc *.Servlet

jad反编译指定已加载类的源码
jad --source-only org.apache.coyote.type.PlaceholderForType

dump已加载类的bytecode到特定目录
dump org.apache.coyote.type.PlaceholderForType

classloader查看classloader的继承树，urls，类加载信息

3、GUI项目
项目地址：https://github.com/4ra1n/shell-analyzer
实时监控目标JVM，一键反编译分析代码，一键查杀内存马

4、日志排查
针对提取的路径，排查访问页面不存在但是日志里的返回200状态码

5、学习资料
https://github.com/Getshell/Mshell

```

