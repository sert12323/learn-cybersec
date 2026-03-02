# Shiro有key无链

JRMP指的是Java远程方法协议（Java Remote Method Protocol）。它是 Java 对象实现远程通信的基础技术，也是Java RMI（Remote Method Invocation，远程方法调用）的底层通信协议。借助JRMP，运行于某个Java虚拟机（JVM）中的对象能够调用另一个JVM中对象的方法，就如同调用本地对象的方法一样便

使用shiro_tool 检测

```
java -jar .\shiro_tool.jar http://127.0.0.1:8081/login
```

![image-20260222233942216](images/image-20260222233942216.png)

删掉利用链

![image-20260222234427614](images/image-20260222234427614.png)

![image-20260222234516444](images/image-20260222234516444.png)



打开端口

![image-20260223110545892](images/image-20260223110545892.png)

生成cb链

![image-20260223110807841](images/image-20260223110807841.png)

攻击机ip加端口

![image-20260223114334593](images/image-20260223114334593.png)