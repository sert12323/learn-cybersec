# JAVA攻防-常规漏洞&SQL注入四类型&XXE引用点&RCE原生&框架URL跳转&URL处理类

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

