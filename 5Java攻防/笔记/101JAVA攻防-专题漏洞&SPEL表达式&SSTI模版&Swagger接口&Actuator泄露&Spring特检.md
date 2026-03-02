# JAVA攻防-专题漏洞&SPEL表达式&SSTI模版&Swagger接口&Actuator泄露&Spring特检

```JAVA
#相关靶场：
https://github.com/bewhale/JavaSec
https://github.com/whgojp/JavaSecLab
https://github.com/j3ers3/Hello-Java-Sec

SQL注入：
-JDBC
1、采用Statement方法拼接SQL语句
2、PrepareStatement会对SQL语句进行预编译，但如果直接采取拼接的方式构造SQL，此时进行预编译也无用。
3、JDBCTemplate是Spring对JDBC的封装，如果使用拼接语句便会产生注入
4、自定义过滤（黑白名单）
安全写法：SQL语句占位符（?） + PrepareStatement预编译

-MyBatis
MyBatis支持两种参数符号，一种是#，另一种是$，#使用预编译，$使用拼接SQL。
1、order by注入：由于使用#{}会将对象转成字符串，形成order by "user" desc造成错误，因此很多研发会采用${}来解决，从而造成注入.
2、like 注入：模糊搜索时，直接使用'%#{q}%' 会报错，部分研发图方便直接改成'%${q}%'从而造成注入.
3、in注入：in之后多个id查询时使用#同样会报错，从而造成注入.

-Hibernate
1、setParameter：预编译
2、username=:username 预编译

-JPA
1、username=:username 预编译
总结
黑盒：正常发现和利用即可
白盒：
1、确定数据库通讯技术
2、确定类型后找调用写法
3、确定写法是否安全（预编译）

XXE注入：
1、XMLReader parse
2、SAXParser parse
/**
 * 审计的函数
 * 1. XMLReader
 * 2. SAXReader
 * 3. DocumentBuilder
 * 4. XMLStreamReader
 * 5. SAXBuilder
 * 6. SAXParser
 * 7. SAXSource
 * 8. TransformerFactory
 * 9. SAXTransformerFactory
 * 10. SchemaFactory
 * 11. Unmarshaller
 * 12. XPathExpression
 */
总结：获取适用以上12种类函数实现，parse后续的可控变量

RCE执行：
1、ProcessBuilder
2、Runtime.getRuntime().exec()
3、ProcessImpl
4、GroovyShell

SSRF
1、URL

URL跳转
1、Spring MVC-redirect ModelAndView
2、HttpServlet-setHeader sendRedirect
3、Spring-ResponseEntity setHeader总结：关注什么技术栈实现源码，看类函数触发可控变量

#其他
XSS,CSRF,文件安全,业务逻辑,冷门漏洞等与其他相似

```

```JAVA
#SSTI模版注入
SSTI(Server Side Template Injection) 服务器模板注入, 服务端接收了用户的输入，将其作为 Web 应用模板内容的一部分，在进行目标编译渲染的过程中，执行了用户插入的恶意内容。
1、Thymeleaf
2、Velocity
3、Freemarker
其他语言参考：https://www.cnblogs.com/bmjoker/p/13508538.html

#SPEL表达式注入
SpEL（Spring Expression Language）表达式注入, 是一种功能强大的表达式语言、用于在运行时查询和操作对象图，方法调用和基本的字符串模板功能。
SpelExpressionParser parseExpression
1、Spring表达式
2、Spring反射绕过

#Swagger UI API框架接口泄露
Swagger是一种用于描述API的开源框架。Swagger接口泄露漏洞是指在使用Swagger描述API时，由于未正确配置访问控制或未实施安全措施，导致API接口被不授权的人员访问和利用，从而导致系统安全风险
1、Apifox导入Swagger解析
2、Apifox测试Swagger接口
发现swagger页面找到利用口:扫描器
利用swagger页面中的接口获取漏洞：手工或工具

#SpringBoot框架
1、框架专扫：
https://github.com/sule01u/SBSCAN
https://gitee.com/team-man/spring-scan
https://github.com/CllmsyK/YYBaby-Spring_Scan
https://github.com/wh1t3zer/SpringBootVul-GUI
发现springboot框架：页面图标和内容
利用springboot框架漏洞点：Actuator 泄露等漏洞

2、Actuator 泄露 
发现Actuator页面找到利用口:扫描器
利用Actuator页面中的接口获取漏洞：
要知道有哪些泄露，如Heapdump泄露泄露，druid jolokia等

Heapdump利用：
JDumpSpider提取器：https://github.com/whwlsfb/JDumpSpider
heapdump_tool提取器：https://github.com/wyzxxz/heapdump_tool
JDumpSpiderGUI提取器：https://github.com/DeEpinGh0st/JDumpSpiderGUI
分析提取出敏感信息（配置帐号密码，接口信息 数据库 短信 云应用等配置）

3、druid jolokia gateway等
环境目前不支持，后续讲到

```

