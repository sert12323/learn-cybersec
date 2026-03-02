## Shiro反序列化案例

Java安全框架，能够用于身份验证、授权、加密和会话管理。

登录勾选 Remember Me  点击登录

![image-20260220105135017](images/image-20260220105135017.png)

抓包看见一长串，这是Shiro 的身份验证

![image-20260220105532625](images/image-20260220105532625.png)

代码中这个是对成功登录进行的处理

![image-20260220113517822](images/image-20260220113517822.png)

设置断点 点击调试

![image-20260220114250711](images/image-20260220114250711.png)

登录请求

1、先系列化后

2、aes加密

3、最后base64



解密请求

1、解码巴涩

2、解密aes

3、反序列化数据



比特流数据 base64编码

![image-20260220121633953](images/image-20260220121633953.png)



利用生成DNSLog

![image-20260220163057103](images/image-20260220163057103.png)

使用ysoserial-all1工具生成poc urldns.txt

```
java -jar .\ysoserial-all1.jar URLDNS "http://whisldntmn.yutu.eu.org" > urldns.txt
```

<img src="images/image-20260220163559199.png" alt="image-20260220163559199" style="zoom:300%;" />

用于加解密的python ai生成的代码

![image-20260220165719865](images/image-20260220165719865.png)

将urldns.txt 和python 放到同级目录

![image-20260220164559968](images/image-20260220164559968.png)

运行python文件

![image-20260220165709695](images/image-20260220165709695.png)

登录抓包

![image-20260220170003512](images/image-20260220170003512.png)

修改Cookie 改成 rememberMe=

![image-20260220170155226](images/image-20260220170155226.png)

![image-20260220170231630](images/image-20260220170231630.png)

## Shiro专业反序列化工具

![image-20260220180526601](images/image-20260220180526601.png)

## 利用java-chains生成 

选则链 填入key

![image-20260220181816777](images/image-20260220181816777.png)

![image-20260220181950785](images/image-20260220181950785.png)