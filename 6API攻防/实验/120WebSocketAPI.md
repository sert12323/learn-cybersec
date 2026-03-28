# WebSocketAPI 简单实验

![image-20260306170404801](images/image-20260306170404801.png)

抓包

![image-20260306170743117](images/image-20260306170743117.png)

尝试跨站语句 拦截 抓包

```
<img src=1 onerror='alert(1)'>
```

![image-20260306171601171](images/image-20260306171601171.png)

这里发现括号被转义了 重新修改正确 修改后全部放行提交

![image-20260306171845611](images/image-20260306171845611.png)

![image-20260306173731970](images/image-20260306173731970.png)