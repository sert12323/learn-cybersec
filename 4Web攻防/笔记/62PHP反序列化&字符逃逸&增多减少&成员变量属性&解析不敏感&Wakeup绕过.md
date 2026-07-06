# Web攻防-PHP反序列化&字符逃逸&增多减少&成员变量属性&解析不敏感&Wakeup绕过

```java
#PHP版本绕过漏洞
CVE-2016-7124（__wakeup绕过）
漏洞编号：CVE-2016-7124
影响版本：PHP 5<5.6.25; PHP 7<7.0.10
漏洞危害：如存在__wakeup方法，调用unserilize()方法前则先调用__wakeup方法，但序列化字符串中表示对象属性个数的值大于真实属性个数时会跳过__wakeup执行
Demo：见CVE.PHP与版本切换演示

案例：
[极客大挑战 2019]PHP
1、下载源码分析，触发flag条件
2、分析会触发调用__wakeup 强制username值
3、利用语言漏洞绕过 CVE-2016-7124
4、构造payload后 修改满足漏洞条件触发
Payload：
select=O%3A4%3A%22Name%22%3A3%3A%7Bs%3A14%3A%22%00Name%00username%22%3Bs%3A5%3A%22admin%22%3Bs%3A14%3A%22%00Name%00password%22%3Bs%3A3%3A%22100%22%3B%7D

#PHP版本绕过机制
影响版本：PHP7.1+
->变量属性不同序列化数据差异
*对象变量属性：
public(公共的):在本类内部、外部类、子类都可以访问
protect(保护的):只有本类或子类或父类中可以访问
private(私人的):只有本类内部可以使用
*序列化数据显示：
public属性序列化的时候格式是正常成员名
private属性序列化的时候格式是%00类名%00成员名
protect属性序列化的时候格式是%00*%00成员名

<?php
class Test
{
    public $sex = "man";
    private $name = "xiaodi";
    protected $age = "33";
}
$t=new Test();
print_r(serialize($t));

->PHP版本导致的属性不同反序列化解析差异
<?php
class test{
    protected $a;
    private $b;
    public function __construct(){
        $this->a = 'abc';
    }
    public function __destruct(){
        echo $this->a;
    }
}

echo serialize(new test());
unserialize('O:4:"test":1:{s:1:"a";s:3:"abc";}');

案例：
[网鼎杯 2020 青龙组]AreUSerialz
1、__destruct()--> process()-->read()
2、绕过is_valid()函数，private和protected属性经过序列化都存在不可打印字符在32-125之外
public $op=2;
public $filename="php://filter/read=convert.base64-encode/resource=flag.php";
public $content;

#PHP字符增多减少逃逸
1、字符变多-str1.php str1-pop.php
运算思路：字符个数多了1个
后续有47个就写47个覆盖后续

2、字符变少-str2.php str2-pop.php
运算思路：字符个数少了1个（5位变4位）
思考写多个就截取后续多少个，如23个等

案例：
CTFSHOW-Web262（逃逸解法）
解题思路：提示有message.php
其中获取msg获取f,m,t 要求token=admin
字符增多通过本地序列化发现62位需要覆盖

```

