# JavaEE应用&Servlet路由技术&JDBC&Mybatis数据库&生命周期

\#JavaEE-HTTP-Servlet&路由&周期

参考：https://blog.csdn.net/qq_52173163/article/details/121110753

1、解释

Servlet是运行在Web服务器或应用服务器上的程序,它是作为来自Web浏览器或其他HTTP客户端的请求和HTTP服务器上的数据库或应用程序之间的中间层。使用Servlet可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，还可以动态创建网页。本章内容详细讲解了web开发的相关内容以及servlet相关内容的配置使用,是JAVAEE开发的重中之重。

 

2、创建和使用Servlet

-创建一个类继承HttpServlet

-web.xml配置Servlet路由

-WebServlet配置Servlet路由

-写入内置方法(init service destroy doget dopost)

 

3、Servlet生命周期

 

4、处理接受和回显

● HttpServletRequest是ServletRequest的子接口

getParameter(name) — String 通过name获得值

getParameterValues — String[ ] 通过name获得多值

 

● HttpServletResponse是ServletResponse的子接口 

setCharacterEncoding() 设置编码格式

setContentType() 设置解析语言

getWriter() 获得一个PrintWriter字符输出流输出数据

PrintWriter 接受符合类型数据

 

\#JavaEE-数据库-JDBC&Mybatis&库

-原生态数据库开发：JDBC

参考：https://www.jianshu.com/p/ed1a59750127

JDBC(Java Database connectivity): 由java提供,用于访问数据库的统一API接口规范.数据库驱动: 由各个数据库厂商提供,用于访问数据库的jar包(JDBC的具体实现),遵循JDBC接口,以便java程序员使用！

1、下载jar

https://mvnrepository.com/

2、引用封装jar

创建lib目录，复制导入后，添加为库

3、注册数据库驱动

Class.forName("com.mysql.jdbc.Driver");

4、建立数据库连接

String url ="jdbc:mysql://localhost:3306/demo01";

Connection connection=DriverManager.getConnection(url,"x","x");

5、创建Statement执行SQL

Statement statement= connection.createStatement();

ResultSet resultSet = statement.executeQuery(sql);

6、结果ResultSet进行提取

while (resultSet.next()){

  int id = resultSet.getInt("id");

  String page_title = resultSet.getString("page_title");

  .......

}

 

安全修复SQL注入：预编译

原理：提前编译好执行逻辑，你注入的语句不会改变原有逻辑！

 

-框架数据库开发：Mybatis

Mybatis是一款优秀的持久层框架,避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程，减少了代码的冗余，减少程序员的操作。