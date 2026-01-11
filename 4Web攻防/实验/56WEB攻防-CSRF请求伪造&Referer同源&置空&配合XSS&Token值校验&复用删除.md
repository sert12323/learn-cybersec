# WEB攻防-CSRF请求伪造&Referer同源&置空&配合XSS&Token值校验&复用删除

## :scissors:CSRF-无检测防护-检测&生成&利用

cloudflared的使用

```
.\cloudflared.exe tunnel --url http://127.0.0.1:80 --http-host-header www.123.com
```



![image-20251228225502572](images/image-20251228225502572.png)

进入到后台添加管理员用bp抓包

<img src="images/image-20251228225903115.png" alt="image-20251228225903115" style="zoom:50%;" />

![image-20251228225929846](images/image-20251228225929846.png)

点击BurpSuite->Engagement tools->Generate CSRF Poc

![image-20251228230009536](images/image-20251228230009536.png)

这里需要勾选，默认已经是打开，自动点击脚本，不需要制作出按钮

![image-20251228230229534](images/image-20251228230229534.png)

新建HTML

![image-20251231163501882](images/image-20251231163501882.png)

将html代码上传到服务器上

```
.\cloudflared.exe tunnel --url http://127.0.0.1:80
```

![image-20251231163835521](images/image-20251231163835521.png)

点击访问==必须是登录过后台的浏览器才行==

![image-20251231170441454](images/image-20251231170441454.png)

成功添加管理员123

![image-20251231170456507](images/image-20251231170456507.png)

第二个例子 创建管理员

![image-20251231194520452](images/image-20251231194520452.png)

抓包

![image-20251231194620089](images/image-20251231194620089.png)

复制html

![image-20251231194641149](images/image-20251231194641149.png)

修改`name`放入服务器

![image-20251231194818842](images/image-20251231194818842.png)

在同一个浏览器中登录网站，完成添加

![image-20251231195504134](images/image-20251231195504134.png)

![image-20251231202552345](images/image-20251231202552345.png)

绕过0：规则匹配绕过问题（代码逻辑不严谨）

1、<meta name="referrer" content="no-referrer">       ==空来源绕过==

2、http://xx.xx.xx.xx/http://xx.xx.xx.xx

绕过1：配合文件上传绕过（严谨使用同源绕过）

绕过2：配合存储XSS绕过（严谨使用同源绕过）