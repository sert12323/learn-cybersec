# ELK专业系统日志分析

快速搭建：

https://mp.weixin.qq.com/s/rakuhGVSoUysso1VD5r0aA

#### 放入文件分析日志

![image-20260320154038421](images/image-20260320154038421.png)

上传文件->设置索引名称

![image-20260320155536047](images/image-20260320155536047.png)

#### 收集日志 -分析日志

Linux

![image-20260320161417161](images/image-20260320161417161.png)

![image-20260320161439434](images/image-20260320161439434.png)

按照顺序输入命令   如果中途出现问题 可以先尝试换一种方法

![image-20260320161808971](images/image-20260320161808971.png)

修改内容

![image-20260320163801616](images/image-20260320163801616.png)

![image-20260320164638961](images/image-20260320164638961.png)

![image-20260320164830298](images/image-20260320164830298.png)

![image-20260320164851695](images/image-20260320164851695.png)

![image-20260320184146700](images/image-20260320184146700.png)

## 恶意文件Yara工具分析

把yara64.exe放到规则文件夹中

![image-20260320190748715](images/image-20260320190748715.png)

```
 .\yara64.exe .\malware_index.yar -r    目录
```

![image-20260320201007131](images/image-20260320201007131.png)

```
利用已知规则测试识别类型：(挖矿&勒索&C2&Webshell等)
yara64.exe malware_index.yar -r 目录

利用已知规则测试识别算法：
yara64.exe crypto_index.yar -r 目录

利用已知规则测试识别加壳：
yara64.exe packers_index.yar -r 目录 

利用已知规则测试识别反沙箱：
yara64.exe antidebug_antivm_index.yar -r 目录 

```

