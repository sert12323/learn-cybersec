# 蓝队技能-应急响应篇&C2后门&权限维持手法&Windows&Linux基线检查&排查封锁清理

```
#C2后门分析处置&权限维持技术处置
#Windows实验：
1、常规C2后门-分析检测
常规C2后门：
-无隐匿手法
删除文件，防火墙阻止程序或IP域名等
-有隐匿手法
CDN，云函数，中转等（溯源和反制）
2、Rookit后门-分析检测
后续课程讲到
3、权限维持技术-分析检测
自启动测试：
REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "backdoor" /t REG_SZ /F /D "C:\shell.exe"
隐藏账户：
net user xiaodi$ xiaodi!@#X123 /add
映像劫持
REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v debugger /t REG_SZ /d "C:\Windows\System32\cmd.exe /c calc"
屏保&登录
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v SCRNSAVE.EXE /t REG_SZ /d "C:\shell.exe" /f
REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /V "Userinit" /t REG_SZ /F /D "C:\shell.exe"
4、Web的内存马-分析检测
后续课程讲到

Linux实验：
1、常规C2后门-分析检测
常规C2后门：
-无隐匿手法
删除文件，防火墙阻止程序或IP域名等
-有隐匿手法
CDN，云函数，中转等（溯源和反制）
2、Rookit后门-分析检测
后续课程
3、权限维持技术-分析检测
4、Web的内存马-分析检测
后续课程

基线检测配合分析项目：
-Windows:
https://github.com/selinuxG/Golin
https://github.com/m-sec-org/d-eyes
https://github.com/FindAllTeam/FindAll
https://github.com/DeEpinGh0st/WindowsBaselineAssistant
-Linux:
https://github.com/selinuxG/Golin
https://github.com/m-sec-org/d-eyes
https://github.com/enomothem/Whoamifuck
https://github.com/Ashro-one/Ashro_linux
https://github.com/sun977/linuxcheckshoot
https://mp.weixin.qq.com/s/fcVMStVXu0BploXWopaHtQ

```

