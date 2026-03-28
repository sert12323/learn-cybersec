# 蓝队技能-流量分析篇&内网隧道工具 类&版本标记&字符特征&FRP&NPS&reGeorg&Venom

```
#蓝队技能-流量分析-内网隧道代理工具
#Frp
https://github.com/fatedier/frp
一个专注于内网穿透的高性能的反向代理应用，支持 TCP、UDP、HTTP、HTTPS 等多种协议，且支持 P2P 通信。可以将内网服务以安全、便捷的方式通过具有公网 IP 节点的中转暴露到公网。
版本：0.3X 0.4X 0.6X
筛选：ip.addr == vpsip&&tcp.len>65
1、未启用TLS加密默认特征：（版本有限制）
{"version":"0.xx.0","hostname":"","os":"windows","arch":"amd64","user":"","privilege_key":"28840a6790af42d3dfcf549ab22172e5","timestamp":1726723109,"run_id":"","metas":{},"pool_count":1}
{"version":"0.xx.0","run_id":"88bcb623b0d06876","server_udp_port":0,"error":""}
{"run_id":"88bcb623b0d06876","privilege_key":"","timestamp":0}
2、启用TLS加密：
frpc:
token = xiaodisec
tls_enable = true
frps：
token = xiaodisec
参考：https://mp.weixin.qq.com/s/APeSpvagBmnrJdjofzh1Yw
添加了tls和加密压缩后的frp在通信过程已经没有了版本信息和会话信息。
不过，在建立连接的过程启用tls前，客户端携带"0x17",请求服务端
后续有"Gryphon"协议，"Gryphon"协议为工控协议，用于车用通讯协定

#Nps
https://github.com/ehang-io/nps
一款轻量级、高性能、功能强大的内网穿透代理服务器。目前支持tcp、udp流量转发，可支持任何tcp、udp上层协议（访问内网网站、本地支付接口调试、ssh访问、远程桌面，内网dns解析等等……），此外还支持内网http代理、内网socks5代理、p2p等，并带有功能强大的web管理端。
版本：0.26.10
筛选：data and ip.addr == vpsip
特征：
连接成功后的特征：
1、明文版本号，MD5加密版本号
2、lib/common/const.go代码定义：
认证成功：服务端向客户端发送sucs，客户端发送main
连接后续使用隧道的特征：
3、通讯后有传输数据特征固定格式：
{"ConnType":"xx","Host":"xx","Crypt":false,"Compress":false,"LocalProxy":false,"RemoteAddr":"xx","Option":{"Timeout":5000000000}}

#Neo-reGeorg
https://github.com/L-codes/Neo-reGeorg
一个旨在积极重构 reGeorg 的项目，目的是：提高可用性，避免特征检测，提高 tunnel 连接安全性，提高传输内容保密性，应对更多的网络环境场景下使用
筛选：http
特征：
1、html注释的字符串
2、post数据包和返回包中存在大量编码后的字符串，同时最开始访问时是不带cookie的。而后面的流量统一带cookie访问
3、固定的数据包格式和UA等

#Venom
https://github.com/Dliv3/Venom
一款为渗透测试人员设计的使用Go开发的多级代理工具。Venom可将多个节点进行连接，然后以节点为跳板，构建多级代理。渗透测试人员可以使用Venom轻松地将网络流量代理到多层内网，并轻松地管理代理节点。
./admin_linux_x64 -lport 8888
agent.exe -rhost 47.239.64.164 -rport 8888
筛选：data and ip.addr == vpsip
特征：
Venom/global/global.go源码：
数据包带有"ABCDEFGH"、"VCMD"

```

