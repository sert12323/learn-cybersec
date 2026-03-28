# 蓝队技能-应急响应篇&近源攻击&Docker镜像&容器分析&Dockfile路径定位&基线扫描

```
#Docker应急
Docker拉取的镜像被攻击者拿下，植入后门或挖矿等恶意应用，那么该如何应急？
1、启动镜像
例子1：docker run -itd --restart=always -e POOL_URL=pool.supportxmr.com:5555 -e POOL_USER=45rfqYG9iNPddvenLpjFskJUhFgqBkdhDeah3X8D8ZJM3KpKqZWCLz3ewLsVd269tZiEyQRV53Ldv2DJb6xeuFokF7SBb1p --name xmrig pmietlicki/xmrig
例子2：
git clone https://github.com/vulhub/vulhub.git
cd vulhub/shiro/CVE-2016-4437
docker compose up -d

2、镜像分析
使用docker history命令查看指定镜像的创建历史，加上--no-trunc，就可以看到全部信息。
docker history pmietlicki/xmrig

使用dfimage从镜像中提取Dockerfile，在这里可以清晰地看到恶意镜像构建的过程
alias dfimage="docker run --rm -v /var/run/docker.sock:/var/run/docker.sock alpine/dfimage"

dfimage -sV=1.36 pmietlicki/xmrig

查看镜像的配置信息
docker inspect --format='{{json .Config}}' pmietlicki/xmrig

获取镜像的运行路径
docker inspect --format='{{.GraphDriver.Data.LowerDir}}' pmietlicki/xmrig

3、处置镜像
查看镜像：docker ps
进入镜像：docker exec -it xxxxx /bin/bash
暂停镜像：docker pause <容器ID>
删除镜像：
docker rm -f <containerId>
docker rmi <IMAGE_NAME>

4、基线检测
https://github.com/anchore/grype/releases
rpm -ivh grype_0.80.0_linux_amd64.rpm
grype <image>
grype <image> --scope all-layers

https://github.com/aquasecurity/trivy
wget https://github.com/aquasecurity/trivy/releases/download/v0.54.1/trivy_0.54.1_Linux-64bit.deb
sudo dpkg -i trivy_0.54.1_Linux-64bit.deb
trivy image xxx:xxxx

#近源攻击
https://mp.weixin.qq.com/s/WBFTtAuyC7PauFZbNBja5Q

```

