# WEB攻防-Python安全&反序列化利用链&PYC文件反编译&格式化字符串安全

\#Python-PYC-反编译文件出源码

pyc文件是py文件编译后生成的字节码文件(byte code)，pyc文件经过python解释器最终会生成机器码运行。因此pyc文件是可以跨平台部署的，类似Java的.class文件，一般py文件改变后，都会重新生成pyc文件。

真题：http://pan.baidu.com/s/1jGpB8DS

安装：pip install uncompyle6

使用：uncompyle6 -o test.py test.pyc  （反编译的对象）

下载：https://github.com/rocky/python-uncompyle6

 

\#Python-反序列化-调用链&魔术方法

各类语言序列化和反序列化函数：

Java： Serializable Externalizable接口、fastjson、jackson、gson、ObjectInputStream.read、ObjectObjectInputStream.readUnshared、XMLDecoder.read、ObjectYaml.loadXStream.fromXML、ObjectMapper.readValue、JSON.parseObject等

PHP： serialize()、 unserialize()

Python：pickle marshal json PyYAML shelve PIL unzip

 

序列化：把类对象转化为字节流或文件

反序列化：将字节流或文件转化为类对象

 

pickle.dump(obj, file) : 将对象序列化后保存到文件

pickle.load(file) : 将文件序列化内容反序列化为对象

pickle.dumps(obj) : 将对象序列化成字符串格式的字节流

pickle.loads(bytes_obj) : 将字符串字节流反序列化为对象

PyYAML yaml.load()

JSON json.loads(s)

marshal

 

魔术方法：

reduce() 反序列化时调用

reduce_ex() 反序列化时调用

setstate() 反序列化时调用（类似于php的isset被设置）

getstate() 序列化时调用

 

1、序列化和反序列化演示-test.py

2、序列化和反序列化形成-test.py

3、序列化和反序列化利用-server.py pop.py

4、序列化和反序列化赛题-[watevrCTF-2019]Pickle Store

黑盒：Python反序列化特征：base64编码 前面gA固定（序列化数据）

测试：直接提交构造的payload测试

 

\#Python-格式化字符串-类魔术方法引用

https://xz.aliyun.com/t/3569

第一种：%操作符

第二种：string.Template

第三种：调用format方法 （可控格式化字符串）

第四种: f-Strings（可控格式化字符串）