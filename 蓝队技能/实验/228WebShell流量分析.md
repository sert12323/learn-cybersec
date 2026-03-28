## WebShell流量分析

![image-20260325154150228](images/image-20260325154150228.png)

### 中国蚁剑

配置代理 连接burp

![image-20260325101811676](images/image-20260325101811676.png)

默认采用连接ua头

![image-20260325113219376](images/image-20260325113219376.png)

Post提交

![image-20260325114955011](images/image-20260325114955011.png)

Base64编码提交的数据（去掉前2位）

![image-20260325115033279](images/image-20260325115033279.png)

把%3D进行url解码 然后去掉前两位进行 base64 解码

![image-20260325115201314](images/image-20260325115201314.png)

编码器决定：@ini_set("display_errors", "0");@set_time_limit(0)

![image-20260325115502346](images/image-20260325115502346.png)

![image-20260325115556751](images/image-20260325115556751.png)

## 冰蝎

ua头   可以把固定库里面的ua头全部收集起来

![image-20260325120643402](images/image-20260325120643402.png)

固定特征

**![image-20260325120803184](images/image-20260325120803184.png)**

可以尝试用默认密钥解密  魔改的不行

![image-20260325122102580](images/image-20260325122102580.png)

## 哥斯拉

成功产生3个流量包  失败产生2个数据包

![image-20260325122537193](images/image-20260325122537193.png)

第一个请求总会发送大量数据，这是配置信息，且请求包内无cookie，服务器响应包无内容，生成一个session，后续请求会带上此session到请求包中的cookie中

![image-20260325122810203](images/image-20260325122810203.png)

![image-20260325122846858](images/image-20260325122846858.png)

第二次 session成功写进去了

![image-20260325122912591](images/image-20260325122912591.png)

强特征：生成的cookie后面有个分号

![image-20260325153850446](images/image-20260325153850446.png)

分析解密流量包，解密返回包和请求包

![image-20260325161031004](images/image-20260325161031004.png)

![image-20260325161529228](images/image-20260325161529228.png)

## ctf题目1

![image-20260325163159316](images/image-20260325163159316.png)

需要：攻击者获得到的服务器用户名  服务器内网ip地址

已知条件：webshell 冰蝎

冰蝎：http协议  post提交



筛选数据包

```
http.request.method=="POST"
```

怀疑1.php

![image-20260325174520136](images/image-20260325174520136.png)

![image-20260325174546527](images/image-20260325174546527.png)

![image-20260325175014318](images/image-20260325175014318.png)

![image-20260325175247624](images/image-20260325175247624.png)

![image-20260325175404783](images/image-20260325175404783.png)

或者直接用工具解密（请求包）

![image-20260325175441698](images/image-20260325175441698.png)

返回包

换成tcp流  寻找一个比较短的包  直到找到有用的包![image-20260325181551136](images/image-20260325181551136.png)

![image-20260325181910419](images/image-20260325181910419.png)

![image-20260325183425601](images/image-20260325183425601.png)

![image-20260325183408637](images/image-20260325183408637.png)

base64解码

![image-20260325183519616](images/image-20260325183519616.png)

![image-20260325183830487](images/image-20260325183830487.png)

将文件保存成phpinfo.php

![image-20260325183955203](images/image-20260325183955203.png)

![image-20260325184044584](images/image-20260325184044584.png)

![image-20260325184109263](images/image-20260325184109263.png)

冰蝎在使用的过程中 基本信息栏位会显示全部信息 找到对应包可以还原phpinfo.php

![image-20260325184318076](images/image-20260325184318076.png)

## ctf题目2

![image-20260325185311913](images/image-20260325185311913.png)

```
http.request.method=="POST"
```

![image-20260325190608350](images/image-20260325190608350.png)

分析出tomcat 上传war包

![image-20260325190721865](images/image-20260325190721865.png)

Java版本

![image-20260325191141315](images/image-20260325191141315.png)

![image-20260325191246495](images/image-20260325191246495.png)

截取前面的包，可以搜索`&` 符号后面可能是密码

![image-20260325191731515](images/image-20260325191731515.png)

没有结果 如果出现报错 需要去除两位（剑蚁特征）

![image-20260325192250854](images/image-20260325192250854.png)

其中在流39中

![image-20260325192634832](images/image-20260325192634832.png)

![image-20260325192844245](images/image-20260325192844245.png)

返回包

![image-20260325193007587](images/image-20260325193007587.png)