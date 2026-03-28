# 蓝队技能-规则开发篇&SuriCata&检测C2&WebShell&隧道代理&结合ChatGPT&正则匹配

```
#蓝队技能-规则开发-Suricata-C2&Webshell&隧道
Windows安装suricata:
https://blog.csdn.net/zengliguang/article/details/142383974


#C2检测 - CS&Sliver
Sliver:
alert tcp any any -> any any (msg: "Sliver HTTP woff request"; flow:to_server,established;content:".woff";http_uri;pcre: "/\/(static|assets|fonts|locales)(.*?)((attribute_text_w01_regular|ZillaSlab-Regular\.subset\.bbc33fb47cf6|ZillaSlab-Bold\.subset\.e96c15f68c68|Inter-Regular|Inter-Medium)\.woff)\?[a-z_]{1,2}=[a-z0-9_]{1,10}/i";sid:1000001;classtype:trojan-activity; rev:1;)

alert tcp any any -> any any (msg: "Sliver HTTP js request"; flow:to_server,established;content:"GET";http_method;nocase;content:".js";http_uri;pcre: "/\/(js|umd|assets|bundle|bundles|scripts|script|javascripts|javascript|jscript)(.*?)((bootstrap|bootstrap.min|jquery.min|jquery|route|app|app.min|array|backbone|script|email)\.js)\?[a-z_]{1,2}=[a-z0-9_]{1,10}/i";sid:1000002;classtype:trojan-activity; rev:1;)

alert tcp any any -> any any (msg: "Sliver HTTP html request&getsessionID"; flow:to_server,established;content:"POST";http_method;nocase;content:".html";http_uri;pcre: "/\/(php|api|upload|actions|rest|v1|oauth2callback|authenticate|oauth2|oauth|auth|database|db|namespaces)(.*?)((login|signin|api|samples|rpc|index|admin|register|sign-up)\.html)\?[a-z_]{1,2}=[a-z0-9_]{1,10}/i";sid:1000003;flowbits:set,name;flowbits:noalert;classtype:trojan-activity; rev:1;)

alert tcp any any <> any any (msg: "Sliver HTTP html response&set-cookie";flow:to_client,established;content:"Set-Cookie";http_header;pcre:"/^Set-Cookie\:\s*(PHPSESSID|SID|SSID|APISID|csrf-state|AWSALBCORS)\=[a-z0-9]{32}\;\s*HttpOnly$/i";sid:1000004;flowbits:isset,name;classtype:trojan-activity;)

alert tcp any any -> any any (msg: "Sliver HTTP php request"; flow:to_server,established;content:"POST";http_method;nocase;content:".php";http_uri;pcre: "/\/(php|api|upload|actions|rest|v1|oauth2callback|authenticate|oauth2|oauth|auth|database|db|namespaces)(.*?)((login|signin|api|samples|rpc|index|admin|register|sign-up)\.php)\?[a-z_]{1,2}=[a-z0-9_]{1,10}/i";sid:1000005;classtype:trojan-activity; rev:1;)

alert tcp any any -> any any (msg: "Sliver HTTP png request"; flow:to_server,established;content:".png";http_uri;pcre: "/\/(static|www|assets|images|icons|image|icon|png)(.*?)((favicon|sample|example)\.png)\?[a-z_]{1,2}=[a-z0-9_]{1,10}/i";sid:1000006;classtype:trojan-activity; rev:1;)

alert tls any any -> any any (msg:"sliver https debian"; ja3.hash; content:"19e29534fd49dd27d09234e639c4057e"; classtype:misc-activity; sid:1001; rev:1;)

alert tls any any -> any any (msg:"sliver https"; ja3.hash; content:"f4febc55ea12b31ae17cfb7e614afda8"; sid:1002;)

CS:
# http-beacon-staging，向c2服务器发起get请求，下载大小约210kb的stager，请求地址符合checksum8规则
# 调用lua检查uri是否符合checksum8规则：计算uri的ascii之和并与256做取余计算，余数为92则符合规则
alert http any any -> any any (gid:3333; sid:30001; rev:1; \
    msg:"http-beacon-checksum8-path-parse"; \
    classtype: http-beacon; \
    flow: established, to_server; \
    urilen:4<>6; \
    luajit:checksum8_check.lua; \
)

# http-beacon上线/心跳请求，匹配敏感路径
alert http any any -> any any (gid:3333; sid:30003; rev:1; \
    msg:"http-beacon-get-data"; \
    classtype: http-beacon; \
    flow:to_server; \
    http.method; content:"GET"; \
    http.accept; content:"*/*"; \
    http.uri; pcre:"/\/ca|\/dpixel|\/__utm.gif|\/pixel.gif|\/g.pixel|\/dot.gif|\/updates.rss|\/fwlink|\/cm|\/cx|\/pixel|\/match|\/visit.js|\/load|\/push|\/ptj|\/j.ad|\/ga.js|\/en_US\/all.js|\/activity|\/IE9CompatViewList.xml/"; \
    http.user_agent; pcre:"/Mozilla\/5.0 \(compatible/"; \
)


# http-beacon执行完下发的命令后，通过post方式向c2服务器发起数据回传请求
alert http any any -> any any (gid:3333; sid:30004; rev:1; \
    msg:"http-beacon-post-data"; \
    classtype: http-beacon; \
    flow:to_server; \
    http.method; content:"POST"; \
    http.uri; content:"/submit.php?id="; \
    http.accept; content:"*/*"; \
    http.content_type; content:"application/octet-stream"; \
    http.connection; content:"keep-alive"; nocase; \
    http.request_body; content:"|00 00 00|"; startswith; \
)


# https-beacon-ja3指纹，client-hello
alert tls any any -> any any (gid:6666; sid:30005; rev:1; \
    msg:"https-beacon-ja3-hash"; \
    classtype: https-beacon; \
    ja3.hash; pcre:"/72a589da586844d7f0818ce684948eea|652358a663590cfc624787f06b82d9ae|4d93395b1c1b9ad28122fb4d09f28c5e|a0e9f5d64349fb13191bc781f81f42e1/"; \
)


# https-beacon-ja3s指纹，server-hello
alert tls any any -> any any (gid:6666; sid:30006; rev:1; \
    msg:"https-beacon-ja3s-hash"; \
    classtype: https-beacon; \
    ja3s.hash; pcre:"/fd4bc6cea4877646ccd62f0792ec0b62|b742b407517bac9536a77a7b0fee28e9/"; \
)


# https-beacon-cert指纹，subject、cert_issuer，默认为空
alert tls any any -> any any (gid:6666; sid:30007; rev:1; \
    msg:"https-beacon-tls-cert-issuer"; \
    classtype: https-beacon; \
    tls.cert_subject; content:"C=, ST=, L=, O=, OU=, CN="; nocase; \
    tls.cert_issuer; content:"C=, ST=, L=, O=, OU=, CN="; nocase; \
    tls_cert_notbefore:2015-05-20T18:26:24; \
    tls_cert_notafter:2025-05-17T18:26:24; \
)


# https-beacon-cert指纹，fingerprint
alert tls any any -> any any (gid:6666; sid:30008; rev:1; \
    msg:"https-beacon-tls-cert-fingerprint"; \
    classtype: https-beacon; \
    tls.cert_fingerprint; content:"6e:ce:5e:ce:41:92:68:3d:2d:84:e2:5b:0b:a7:e0:4f:9c:b7:eb:7c" ;\
)

# https-beacon-cert指纹，serialNumber
alert tls any any -> any any (gid:6666; sid:30009; rev:1; \
    msg:"https-beacon-tls-cert-fingerprint"; \
    classtype: https-beacon; \
    tls.cert_serial; content:"08:BB:00:EE"; \
)


# dns-beacon，匹配dns-beacon发起上线/心跳请求后，c2服务器的返回包
# Type: A, Class: IN, 0.0.0.0
alert dns any any -> any any (gid:9999; sid:30010; rev:1; \
    msg:"dns-beacon-live-response"; \
    classtype: dns-beacon; \
    content:"|00 01 00 01 00 00 00|"; \
    content:"|00 00 00 00|"; endswith; \
)


# dns-beacon，匹配dns-beacon发起上线/心跳请求后，c2服务器的返回包，选择后续使用A记录
# Type: A, Class: IN, 0.0.0.241
alert dns any any -> any any (gid:9999; sid:30011; rev:1; \
    msg:"dns-beacon-live-response"; \
    classtype: dns-beacon; \
    content:"|00 01 00 01 00 00 00|"; \
    content:"|00 00 00 f1|"; nocase; endswith; \
)


# dns-beacon，匹配dns-beacon发起上线/心跳请求后，c2服务器的返回包，选择后续使用TXT记录
# Type: A, Class: IN, 0.0.0.243
alert dns any any -> any any (gid:9999; sid:30012; rev:1; \
    msg:"dns-beacon-live-response"; \
    classtype: dns-beacon; \
    content:"|00 01 00 01 00 00 00|"; \
    content:"|00 00 00 f3|"; nocase; endswith; \
)


# dns-beacon，匹配dns-beacon发起上线/心跳请求后，c2服务器的返回包，选择后续使用AAAA记录
# Type: A, Class: IN, 0.0.0.245
alert dns any any -> any any (gid:9999; sid:30013; rev:1; \
    msg:"dns-beacon-live-response"; \
    classtype: dns-beacon; \
    content:"|00 01 00 01 00 00 00|"; \
    content:"|00 00 00 f5|"; nocase; endswith; \
)


# dns-beacon，匹配dns-beacon发起元数据提交请求后，c2服务器的确认返回包
# 以www开头0.0.0.0结尾的A记录查询返回包
# Type: A, Class: IN, 0.0.0.0
alert udp any any -> any any (gid:9999; sid:30014; rev:1; \
    msg:"dns-beacon-metadata-response"; \
    classtype: dns-beacon; \
    flow:to_client; \
    content:"www"; \
    content:"|00 01 00 01 00 00 00|"; \
    content:"|00 00 00 00|"; nocase; endswith; \
)


# dns-beacon，匹配dns-beacon使用AAAA、TXT方式向c2服务器发起payload下载请求后，c2服务器的返回包
# www6 ==> AAAA 、api ==> TXT 
# Type: A, Class: IN, 0.0.0.80
alert udp any any -> any any (gid:9999; sid:30015; rev:1; \
    msg:"dns-beacon-getpayload-response"; \
    classtype: dns-beacon; \
    flow:to_client; \
    pcre:"/www6|api/"; \
    content:"|00 01 00 01 00 00 00|"; \
    content:"|00 00 00 50|"; endswith; \
)


# dns-beacon，匹配dns-beacon使用A方式向c2服务器发起payload下载请求后，c2服务器的返回包
# cdn ==> A
# Type: A, Class: IN, 0.0.0.64
alert udp any any -> any any (gid:9999; sid:30016; rev:1; \
    msg:"dns-beacon-getpayload-response"; \
    classtype: dns-beacon; \
    flow:to_client; \
    pcre:"/cdn/"; \
    content:"|00 01 00 01 00 00 00|"; \
    content:"|00 00 00 40|"; endswith; \
)


# dns-beacon，匹配dns-beacon使用执行完payload后，向c2服务器执行结果数据，c2服务器的返回包
# post ==> put_output
# Type: A, Class: IN, 0.0.0.00
alert udp any any -> any any (gid:9999; sid:30017; rev:1; \
    msg:"dns-beacon-output-response"; \
    classtype: dns-beacon; \
    flow:to_client; \
    content:"post"; \
    content:"|00 01 00 01 00 00 00|"; \
    content:"|00 00 00 00|"; nocase; endswith; \
)

#内网隧道工具-FRP&NPS
alert tcp any any -> $HOME_NET any (msg:"frp find"; content:"{\"version\":\""; pcre:"/\"version\":\"0\.\d{2}\.\d{1}\"/"; nocase; sid:1000042; rev:1;)

alert tcp any any -> $HOME_NET any (msg:"frp find"; content:"{\"run_id\":\""; nocase; sid:1000044; rev:1;)

alert tcp any any -> $HOME_NET any (msg:"nps find"; content:"sucs"; nocase; sid:1000053; rev:1;)

#Webshell-蚁剑&冰蝎&哥斯拉
alert http any any -> $HOME_NET any (msg:"antSword find"; content:"User-Agent|3a| antSword/v2.1"; http_header; sid:1000009; rev:1;)

alert http any any -> $HOME_NET any (msg:"behinder find"; content:"Accept:"; http_header; content:"text/html"; http_header; content:"application/xhtml+xml"; http_header; content:"application/xml"; http_header; content:"q=0.9"; http_header; content:"image/webp"; http_header; content:"image/apng"; http_header; content:"*/*"; http_header; content:"q=0.8"; http_header; content:"application/signed-exchange"; http_header; content:"v=b3"; http_header; content:"Accept-Language: zh-CN"; http_header; content:"q=0.9"; http_header; content:"en-US"; http_header; content:"q=0.8"; http_header; sid:1000047; rev:3;)

alert http any any -> $HOME_NET any (msg:"godzilla find"; content:"Accept:"; http_header; content:"text/html"; http_header; content:"application/xhtml+xml"; http_header; content:"application/xml"; http_header; content:"q=0.9"; http_header; content:"image/webp"; http_header; content:"*/*"; http_header; content:"q=0.8"; http_header; sid:1000099; rev:1;)


```

