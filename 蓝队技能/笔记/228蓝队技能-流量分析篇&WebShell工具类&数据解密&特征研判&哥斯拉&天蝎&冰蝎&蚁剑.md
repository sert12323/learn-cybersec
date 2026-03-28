# 蓝队技能-流量分析篇&WebShell工具类&数据解密&特征研判&哥斯拉&天蝎&冰蝎&蚁剑

```
#蓝队技能-流量分析-WebShell工具
WebShell工具类：菜刀，蚁剑，冰蝎，天蝎，哥斯拉
菜刀：
https://mp.weixin.qq.com/s/SjUMoY6EafQzAb3RNWNkpg

蚁剑：（default,rot13）
0、Post提交
1、User-Agent: antSword/v2.1
2、编码器决定：@ini_set("display_errors", "0");@set_time_limit(0);
3、Base64编码提交的数据（去掉前2位）

冰蝎：
1、User-Agent: Mozilla/5.0 (Windows NT 6.1; Trident/7.0; rv:11.0) like Gecko
2、Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
3、Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
4、部分3.0以后的子版本至4.0以后增加了referer参数，末尾文件名随机大小写

天蝎：
1、X-Forwarded-For: 
2、User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36 Edg/89.0.774.57
3、Content-Type: application/octet-stream

哥斯拉：（PHP_EVAL_XOR_BASE64）
0、eval(base64_decode(strrev(urldecode('
1、强特征：链接失败情况下2个流量包，成功为3个流量包
2、第一个请求总会发送大量数据，这是配置信息，且请求包内无cookie，服务器响应包无内容，生成一个session，后续请求会带上此session到请求包中的cookie中
3、强特征：生成的cookie后面有个分号
4、Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
5、Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2

工具解密定性：（天蝎，冰蝎，哥斯拉）
知道密钥（工具内置的默认密钥，魔改二开发生的算法变化或密钥修改，则失效）
https://github.com/abc123info/BlueTeamTools


#WireShark使用
1.IP地址
ip.addr==IP 针对某个IP过滤
ip.src==IP 源IP过滤
ip.dst==IP 目的IP过滤过滤目的ip    

2.端口：
tcp.port==端口号 显示主机端口的数据包
tcp.srcport==端口号 显示源主机端口的数据包
tcp.dstport==端口号 显示目的主机端口的数据包

3.协议：
tcp、udp、http、arp、icmp、smtp、pop、telnet、ssh、rdp等等
小写直接输入，回车

4.GET,POST过滤，大写
http.request.method=="GET"
http.request.method=="POST"

5.逻辑运算符：and、or、|| 、&等等
ip.src==IP and http 过滤源地址并且是http协议的    

6.特定字符过滤
http contains "Content-Type: "
http.request.uri=="/pikachu/vul/unsafeupload/uploads/1.php" 

面试题型：
#冰蝎数据包
1、找到流量数据包
http.request.method=="POST"
2、请求回显流量解密
AES-BASE64
默认密钥：e45e329feb5d925b
http://tools.bugscaner.com/cryptoaes/
#蚁剑数据包
1、找到流量数据包
http.request.method=="POST"
2、请求回显流量解密

```

