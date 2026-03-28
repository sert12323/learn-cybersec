## NIDS-Snort

安装教程[(Ubuntu22.04上Snort3的安装与基本配置-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2616846))

在snort.conf文件中设置 要包含的规则

<img src="images/image-20260321124625602.png" alt="image-20260321124625602" style="zoom:67%;" />

1、ICMP协议流量警告

开启监听

```
snort -i eth0 -c /etc/snort/snort.conf -A fast -l /var/log/snort
```

![image-20260321124901586](images/image-20260321124901586.png)

ping 主机 ip

![image-20260321125050918](images/image-20260321125050918.png)

```
cat /var/log/snort/alert
```

![image-20260321125523385](images/image-20260321125523385.png)

新建规则

![image-20260321125640530](images/image-20260321125640530.png)

![image-20260321125909167](images/image-20260321125909167.png)

在配置文件中新建包含

![image-20260321130156009](images/image-20260321130156009.png)

![image-20260321130834530](images/image-20260321130834530.png)

警告sql注入规则

```
alert tcp any any -> any 80 (msg:"Potential SQL Injection"; sid:1000001; rev:1; content:"' or '1'='1"; nocase;)
```

![image-20260321132942830](images/image-20260321132942830.png)

自定义规则

![image-20260321155859832](images/image-20260321155859832.png)

找到进程

![image-20260321160010380](images/image-20260321160010380.png)

用proxifer 代理应用流量

![image-20260321160023528](images/image-20260321160023528.png)

使用工具，抓包

![image-20260321160120210](images/image-20260321160120210.png)

寻找特征

![image-20260321160237431](images/image-20260321160237431.png)

![image-20260321160258099](images/image-20260321160258099.png)

最快的办法交给ai   （bu'yi'd）

![image-20260321160444877](images/image-20260321160444877.png)

![image-20260321160618044](images/image-20260321160618044.png)

![image-20260321160633131](images/image-20260321160633131.png)

![image-20260321160711420](images/image-20260321160711420.png)