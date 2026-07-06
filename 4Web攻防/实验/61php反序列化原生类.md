#生成原生类： 查找原生类

```
<?php
$classes = get_declared_classes();
foreach ($classes as $class) {
    $methods = get_class_methods($class);
    foreach ($methods as $method) {
        if (in_array($method, array(
			'__construct',
            '__destruct',
            '__toString',
            '__wakeup',
            '__call',
            '__callStatic',
            '__get',
            '__set',
            '__isset',
            '__unset',
            '__invoke',
            '__set_state'
        ))) {
            print $class . '::' . $method . "\n";
        }
    }
}
```

原生类笔记参考

https://xz.aliyun.com/news/8792
https://www.anquanke.com/post/id/264823
https://blog.csdn.net/cjdgg/article/details/115314651