# 蓝队技能-应急响应篇&Rookit后门&进程提取&网络发现&隐藏技术&Linux杀毒&OpenArk

```
Windows-Rootkit演示：
演示项目：https://bytecode77.com/r77-rootkit
进程隐藏，网络隐藏，文件隐藏，注册表，启动项等
安装使用：见本期193天课程

应急项目：
火绒剑（可检测到隐藏进程）
unhide （可检测到隐藏进程，网络隐藏）
https://www.unhide-forensics.info/

应急思路：
1、如果识别到什么项目rookit可以下载工具还原
2、识别不到的情况下采用发现隐藏进程网络等项目

Linux-Rootkit演示：
演示项目：https://github.com/f0rb1dd3n/Reptile
进程隐藏，网络隐藏，文件隐藏，隐藏的引导持久性等
安装使用：见本期195天课程

应急项目：
1、unhide （可检测到隐藏进程，网络隐藏）
https://www.unhide-forensics.info/
安装使用：https://mp.weixin.qq.com/s/LnqArTrAytMKtitGvplNug
一个小巧的网络取证工具，能够发现那些借助rootkit、LKM及其它技术隐藏的进程和TCP/UDP端口。

2、clamav（可检测到各类系统恶意后门文件等）
https://www.clamav.net/
开源的反病毒引擎，用于检测和删除各种恶意软件，包括病毒、木马、间谍软件等
安装使用：
https://mp.weixin.qq.com/s/QL44BbioDcYt6B17L-pTfQ
扫码全部内容：
clamscan /
只扫描/bin/目录：
clamscan /bin/
递归扫描home目录，将病毒文件删除，并且记录日志：
clamscan -r -i /home  --remove  -l /var/log/clamav.log  
扫描指定目录，然后将感染文件移动到指定目录，并记录日志：
clamscan -r -i /home  --move=/tmp/clamav -l /var/log/clamav.log 
常需要扫描的重点目录：
clamscan -r  -i /etc --max-dir-recursion=5 -l /var/log/clamav-etc.logclamscan -r  -i /bin --max-dir-recursion=5 -l /var/log/clamav-bin.logclamscan -r  -i /usr --max-dir-recursion=5 -l /var/log/clamav-usr.logclamscan -r  -i /var --max-dir-recursion=5 -l /var/log/clamav-var.log
查看病毒文件：
cat /var/log/clamav-bin.log | grep "FOUND" 

3、https://www.chkrootkit.org/
4、https://rkhunter.sourceforge.net/
安装：
tar -zxf rkhunter-1.4.6.tar.gz 
cd rkhunter-1.4.6
./installer.sh --install
使用：
rkhunter --check 
rkhunter -c --rwo #（report-warnings-only，只显示报警信息） 
rkhunter --check --skip-keypress  #后台运行，无需手工确认

综合分析工具箱：
https://github.com/BlackINT3/OpenArk
```

