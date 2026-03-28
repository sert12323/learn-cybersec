# 蓝队技能-应急响应篇&ELK系统&日志采集分析&Yara规则&样本识别&特征提取&规则编写

```JAVA
#ELK专业系统日志分析
Elasticsearch:用于存储收集到的日志信息；
Logstash:用于收集日志转发给Elasticsearch；
Kibana:通过Web端的可视化界面来查看日志。
快速搭建：
https://mp.weixin.qq.com/s/rakuhGVSoUysso1VD5r0aA
三种模式：上传文件，特定分析，代理加入
演示：上传日志文件分析，特定部署导入分析
解决：从各种日志的采集到导入平台后索引分析

#恶意文件Yara工具分析
Yara下载：
https://github.com/VirusTotal/yara

恶意样本：
https://github.com/ytisf/theZoo

检测规则：
https://github.com/Yara-Rules/rules
https://github.com/JerryLinLinLin/Huorong-ATP-Rules
https://github.com/m-sec-org/d-eyes/tree/master/yaraRules
规则分11大类：
Antidebug_AntiVM：反调试/反沙箱类yara规则
Crypto：加密类yara规则
CVE_Rules：CVE漏洞利用类yara规则
email：恶意邮件类yara规则
Exploit-Kits：EK类yara规则
Malicious_Documents：恶意文档类yara规则
malware：恶意软件类yara规则
Mobile_Malware：移动恶意软件类yara规则
Packers：加壳类yara规则
utils：通用类yara规则
Webshells：Webshell类yara规则

检测范围：
1、样本文件 
2、内存数据 
3、网络流量

特征提取：
1、多个样本同时对比筛选通用的数据
2、要根据样本的应用(分类,走的协议,文件头固定等)

规则开发：
参考：https://mp.weixin.qq.com/s/Ng8My7P3yIvDtEriacrZYw
-Yara规则内容支持字符串、正则表达式、十六进制进行匹配。
字符串：定义一个变量 $a = "字符串内容"
正则表达式：定义一个变量 $a = /正则表达式内容/
十六进制：定义一个变量 $a = {十六进制内容}
-Yara规则条件
and：与 or：或 not：非
all of them：所有条件匹配即告警
any of them：有一个条件匹配即告警
$a and $b and $c：abc同时匹配即告警
($a and $b) or $c：匹配a和b或c即告警
-Yara规则常用修饰符
nocase：不区分大小写
base64：base64字符串
xor：异或字符串
wide：宽字符


例子：
利用已知规则测试识别类型：(挖矿&勒索&C2&Webshell等)
yara64.exe malware_index.yar -r E:\蓝队\224\test

利用已知规则测试识别算法：
yara64.exe crypto_index.yar -r E:\蓝队\224\test

利用已知规则测试识别加壳：
yara64.exe packers_index.yar -r E:\蓝队\224\test

利用已知规则测试识别反沙箱：
yara64.exe antidebug_antivm_index.yar -r E:\蓝队\224\test

利用自写规则测试识别挖矿：
检测原理：
1、文件头
2、关键字（域名，协议，特征）
yara64.exe miner.yar -r E:\蓝队\224\test

rule xmrig_find : miner {
	meta:
		author = "xiaodisec"
		date = "2024-09-05"
	strings:
		$hex = {4D 5A}
		$a = "stratum"
		$b = "xmrig"
		$c = "pool"
	condition:
		all of them
}

利用自写规则测试识别内存马：
检测原理：
1、类名
2、classLoader
yara64.exe mem_find.yar PID

rule mem_find : memshell {
	meta:
		author = "xiaodisec"
		date = "2024-09-05"
	strings:
		$a = "org.apache.coyote.type.MapLikeType"
		$b = "org.apache.coyote.deser"
		$c = "org.apache.coyote.exc"
	condition:
		($a and $b) or $c
}

```

