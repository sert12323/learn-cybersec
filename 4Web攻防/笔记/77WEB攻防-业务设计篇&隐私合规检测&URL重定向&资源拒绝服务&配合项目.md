# WEB攻防-业务设计篇&隐私合规检测&URL重定向&资源拒绝服务&配合项目

\#隐私合规-判断规则&检测项目

对象：APP 小程序等

具体：后续APP安全课程

-https://appscan.ly.com/

-https://github.com/bytedance/appshark

某SRC规则参考：https://mp.weixin.qq.com/s/tgEZth3TFyyis_EBrQ5PhQ

 

\#URL重定向-检测判断&钓鱼配合

URL 重定向漏洞（URL redirection vulnerability），是一种常见的 Web 安全漏洞，由于网站 URL 重定向功能设计不当，没有验证跳转的目标 URL 是否合法，用户可通过此漏洞跳转到任意网站，这会导致可通过该网站跳转到存在木马、病毒的网站或者钓鱼网站，国外大厂的一个任意URL跳转都500$、1000$了，国内看运气~ 

 

黑盒看业务：

用户登录、统一身份认证处，认证完后会跳转

用户分享、收藏内容过后，会跳转

跨站点认证、授权后，会跳转

站内点击其它网址链接时，会跳转

 

黑盒看参数名：

redirect

redirect_to

redirect_url

url

jump

jump_to

target

to

link

linkto

domain

 

白盒看代码块：

Java：response.sendRedirect(request.getParameter("url"))

PHP:

$redirect_url = $_GET['url'];

header("Location: " . $redirect_url)

.NET:

string redirect_url = request.QueryString["url"];

Response.Redirect(redirect_url);

Django:

redirect_url = request.GET.get("url")

HttpResponseRedirect(redirect_url)

Flask:

redirect_url = request.form['url']

redirect(redirect_url)

Rails:

redirect_to params[:url]

 

单斜线"/"绕过 https://www.landgrey.me/redirect.php?url=/www.evil.com 2. 缺少协议绕过 https://www.landgrey.me/redirect.php?url=//www.evil.com 3. 多斜线"/"前缀绕过 https://www.landgrey.me/redirect.php?url=///www.evil.com https://www.landgrey.me/redirect.php?url=www.evil.com 4. 利用"@"符号绕过 https://www.landgrey.me/redirect.php?url=https://www.landgrey.me@www.evil.com 5. 利用反斜线"\"绕过 https://www.landgrey.me/redirect.php?url=https://www.evil.com\www.landgrey.me 6. 利用"#"符号绕过 https://www.landgrey.me/redirect.php?url=https://www.evil.com#www.landgrey.me 7. 利用"?"号绕过 https://www.landgrey.me/redirect.php?url=https://www.evil.com?www.landgrey.me 8. 利用"\\"绕过 https://www.landgrey.me/redirect.php?url=https://www.evil.com\\www.landgrey.me 9. 利用"."绕过 https://www.landgrey.me/redirect.php?url=.evil      (可能会跳转到www.landgrey.me.evil域名) https://www.landgrey.me/redirect.php?url=.evil.com    (可能会跳转到evil.com域名) 10.重复特殊字符绕过 https://www.landgrey.me/redirect.php?url=///www.evil.com//.. https://www.landgrey.me/redirect.php?url=www.evil.com//..

 

\#资源拒绝服务-加载受控&处理受控

功能1：验证码或图片显示自定义大小

功能2：上传压缩包解压循环资源占用（压缩包炸弹）