# 蓝队技能-应急响应篇&Web内存马查杀&Spring框架型&中间件型&JVM分析&Class提取

```java
目前Java内存马具体分类：
1、传统Web应用型内存马：
Servlet型内存马：动态注册Servlet及映射路由。
Filter型内存马：动态注册Filter及映射路由。
Listener型内存马：动态注册Listener中的处理逻辑。

2、框架型内存马：
SpringController型内存马：动态注册Controller及映射路由。
SpringInterceptor型内存马：动态注册Interceptor及映射路由。
Spring Webflux型内存马：动态注册WebFilter及映射路由。

3、中间件型内存马：
Tomcat Valve型内存马：动态注册Valve。
Tomcat其他类型（如Upgrade、Executor、Poller）的内存马：
这些内存马会动态替换或添加全局组件。

4、其他中间件（如Grizzly）的内存马：(未讲到)
动态注册Filter及映射路由。其他内存马：
WebSocket型内存马：动态注册WebSocket路由及处理逻辑。
Tomcat JSP型内存马：动态注册Tomcat JSP管理逻辑并实现驻留。
线程型内存马：启动一个无法被杀死的线程。
RMI型内存马：动态启动一个RMI Registry。

5、Agent型内存马：(未讲到)
通过Hook并修改关键方法添加恶意逻辑。Agent型内存马在现代webshell管理工具中有广泛实现。

-Filter/Servlet内存马(上节课)
-Spring框架内存马
-Netty/WebFlux框架内存马
-TomcatValve中间件内存马

->启动：
d:\jdk1.8.0_112\bin\java.exe -jar springinject-0.0.1-SNAPSHOT.jar
d:\jdk1.8.0_112\bin\java.exe -jar WebFluxMem-0.0.1-SNAPSHOT.jar

->检测：
项目1：（实时监控提取方便调试）
能排查几乎所有内存马技术
https://github.com/alibaba/arthas
d:\jdk1.8.0_112\bin\java.exe -jar arthas-boot.jar
sc * | grep Controller
sc * | grep Interceptor
jad --source-only com.example.springinject.demos.web.myInjectController3
jad --source-only com.example.springinject.demos.web.testInjectInterceptor

sc * | grep Memshell
jad --source-only com.example.webfluxmem.WebFluxFilterMemshell

sc *.Valve
jad --source-only org.apache.jsp.valve_jsp$1

项目2：（自动提取需要自己排查）
https://github.com/4ra1n/shell-analyzer
能排查：Filter，Servlet，Listener，Valve类型

项目3：（自动提取排查）
https://github.com/c0ny1/java-memshell-scanner
能排查：Filter，Servlet，Listener类型

项目4：（自动提取需要自己排查）
能排查几乎主流内存马技术
https://github.com/LandGrey/copagent/raw/release/cop.jar 
d:\jdk1.8.0_112\bin\java.exe -jar cop.jar

->清除：
Jar包中删除对应class文件

蓝队内存马大概排查思路：
1、弄清楚当前环境的组织架构：中间件 框架等
2、对应查找可能类型的内存马技术
如：框架 springboot
3、基于上述信息想到用何种项目工具便于分析、
如：项目 arthas （gui 脚本其他项目不支持）
4、接下来怎么开始操作呢
springboot 
SpringController型内存马：动态注册Controller及映射路由。
SpringInterceptor型内存马：动态注册Interceptor及映射路由。
Spring Webflux型内存马：动态注册WebFilter及映射路由。
arthas使用语法：
sc * | grep Controller
sc * | grep Interceptor
arthas使用语法：
关键字筛选 常规shell触发代码关键字
memshell shell os  runtime 

注意：
不同内存马类型查杀arthas检测：有的是基于内存马固定的后缀，名字，文件中的常见关键字去找到可疑类然后去定性

```

