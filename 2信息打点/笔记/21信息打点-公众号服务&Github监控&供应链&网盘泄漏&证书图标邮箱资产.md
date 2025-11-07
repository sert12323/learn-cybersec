# 21信息打点-公众号服务&Github监控&供应链&网盘泄漏&证书图标邮箱资产

\#微信公众号-获取&三方服务

1、获取微信公众号途径

https://weixin.sogou.com/

2、微信公众号有无第三方服务

 

\#Github监控-开发&配置&源码

  目标中开发人员或者托管公司上传的项目存在源码泄漏或配置信息（密码密匙等），人员数据库等敏感信息，找到多个脆弱点。

1、人员&域名&邮箱等筛选

eg：xxx.cn password in:file

https://gitee.com/

https://github.com/

https://www.huzhan.com/

GITHUB资源搜索：

in:name test        #仓库标题搜索含有关键字 

in:descripton test     #仓库描述搜索含有关键字 

in:readme test       #Readme文件搜素含有关键字 

stars:>3000 test      #stars数量大于3000的搜索关键字 

stars:1000..3000 test    #stars数量大于1000小于3000的搜索关键字 forks:>1000 test      #forks数量大于1000的搜索关键字 

forks:1000..3000 test    #forks数量大于1000小于3000的搜索关键字 size:>=5000 test      #指定仓库大于5000k(5M)的搜索关键字 pushed:>2019-02-12 test   #发布时间大于2019-02-12的搜索关键字 created:>2019-02-12 test  #创建时间大于2019-02-12的搜索关键字 user:test          #用户名搜素 

license:apache-2.0 test   #明确仓库的 LICENSE 搜索关键字 language:java test     #在java语言的代码中搜索关键字 

user:test in:name test   #组合搜索,用户名test的标题含有test的

关键字配合谷歌搜索：

site:Github.com smtp  

site:Github.com smtp @qq.com  

site:Github.com smtp @126.com  

site:Github.com smtp @163.com  

site:Github.com smtp @sina.com.cn 

site:Github.com smtp password 

site:Github.com String password smtp

 

2、语法固定长期后续监控新泄露

-基于关键字监控

-基于项目规则监控

https://github.com/madneal/gshark

https://github.com/NHPT/FireEyeGoldCrystal

https://github.com/Explorer1092/Github-Monitor

 

\#网盘资源搜索-全局文件机密

主要就是查看网盘中是否存有目标的敏感文件

如：企业招标，人员信息，业务产品，应用源码等

 

\#敏感目录文件-目录扫描&爬虫

-后续会详细讲到各类工具项目

 

\#网络空间进阶-证书&图标&邮箱

-证书资产

fofa quake hunter

-ICO资产

fofa quake hunter

-邮箱资产

https://hunter.io/

 

\#实战案例四则-技术分享打击方位

案例1--招标平台二级跳

案例2--爱企查隐藏的惊喜

案例3--邮箱爆破到内网

案例4--不靠系统漏洞，从外网获取域控