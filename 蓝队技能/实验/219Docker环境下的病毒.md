top 查看

![image-20260314155133952](images/image-20260314155133952.png)

尝试删除进程

![image-20260314155837438](images/image-20260314155837438.png)

病毒会重启启动 更新pid

![image-20260314155853267](images/image-20260314155853267.png)

ps -aux 查看 所有进程信息查看 **所有进程信息**

<img src="images/image-20260314160050100.png" alt="image-20260314160050100" style="zoom:200%;" />

./xmrig 说明在当前目录 查看当前目录没有发现 xmrig

<img src="images/image-20260314160422641.png" alt="image-20260314160422641" style="zoom:200%;" />

ls -al 查看隐藏文件没有发现

![image-20260314160514341](images/image-20260314160514341.png)

systemd 典型的docker启动 ,启动后会出现

![image-20260314160616259](images/image-20260314160616259.png)

docker top 查看当前运行镜像

![image-20260314160850479](images/image-20260314160850479.png)

使用docker history命令查看指定镜像的创建历史 docker镜像加载的全部命令

```
docker history pmietlicki/xmrig 
```

```
docker history pmietlicki/xmrig | grep xmrig
```

可能是这个镜像跟刚才cpu进程相关

![image-20260314161149805](images/image-20260314161149805.png)

查看路径

![image-20260314161417485](images/image-20260314161417485.png)

无法直接进入文件夹

![image-20260314161514026](images/image-20260314161514026.png)

确认id号 docker ps

![image-20260314161619444](images/image-20260314161619444.png)

```
查看镜像：docker ps
进入镜像：docker exec -it xxxxx /bin/bash
暂停镜像：docker pause <容器ID>
删除镜像：
docker rm -f <containerId>
docker rmi <IMAGE_NAME>
```

![image-20260314161718856](images/image-20260314161718856.png)

成功进入

![image-20260314161818967](images/image-20260314161818967.png)

ps 进程1 和 7 与病毒相关

![image-20260314162145218](images/image-20260314162145218.png)

尝试删除进程

![image-20260314162132476](images/image-20260314162132476.png)

查看任务管理器 发现病毒进程还在运行

![image-20260314162331534](images/image-20260314162331534.png)

给一条很长的 Docker 命令起一个 **简写名字 `dfimage`** 分析 Docker 镜像每一层占用多少空间

```
alias dfimage="docker run --rm -v /var/run/docker.sock:/var/run/docker.sock alpine/dfimage"
dfimage -sV=1.36 pmietlicki/xmrig
```

可以看到镜像的环境变量

![image-20260314164202809](images/image-20260314164202809.png)

查看镜像的配置信息

```
docker inspect --format='{{json .Config}}' pmietlicki/xmrig
```

![image-20260314163922890](images/image-20260314163922890.png)

获取镜像的运行路径    (获取的docker 的磁盘路径)

```
docker inspect --format='{{.GraphDriver.Data.LowerDir}}' pmietlicki/xmrig
```

![image-20260314164425954](images/image-20260314164425954.png)

这里有多个路径 一个一个找 找到 xmrig

![image-20260314165758481](images/image-20260314165758481.png)

其中在这个路径下找到了  挨个排除每个路径

![image-20260314165912155](images/image-20260314165912155.png)

![image-20260314165921855](images/image-20260314165921855.png)

![image-20260314165929904](images/image-20260314165929904.png)

![image-20260314170233203](images/image-20260314170233203.png)

删掉文件

![image-20260314170253748](images/image-20260314170253748.png)

![image-20260314170710351](images/image-20260314170710351.png)

kill 1 和7

![image-20260314170747471](images/image-20260314170747471.png)

病毒成功结束进程

![image-20260314170806198](images/image-20260314170806198.png)

重新启动docker  病毒不在启动

![image-20260314171003282](images/image-20260314171003282.png)



## grype 检查docker 入口点 修复方案

```
https://github.com/anchore/grype/releases
rpm -ivh grype_0.80.0_linux_amd64.rpm
grype <image>
grype <image> --scope all-layers

```

![image-20260314183314832](images/image-20260314183314832.png)



## trivy

```
https://github.com/aquasecurity/trivy
wget https://github.com/aquasecurity/trivy/releases/download/v0.54.1/trivy_0.54.1_Linux-64bit.deb
sudo dpkg -i trivy_0.54.1_Linux-64bit.deb
trivy image xxx:xxxx

```

![image-20260314183902704](images/image-20260314183902704.png)

![image-20260314184009163](images/image-20260314184009163.png)