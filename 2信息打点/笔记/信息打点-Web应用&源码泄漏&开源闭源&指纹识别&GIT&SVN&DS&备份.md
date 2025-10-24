# 信息打点-Web应用&源码泄漏&开源闭源&指纹识别&GIT&SVN&DS&备份

\#后端-开源-指纹识别-源码下载

CMS识别见上述项目

| **标签** | **名称**         | **地址**                                   |
| -------- | ---------------- | ------------------------------------------ |
| 指纹识别 | 在线cms指纹识别  | http://whatweb.bugscaner.com/look/         |
| 指纹识别 | Wappalyzer       | https://github.com/AliasIO/wappalyzer      |
| 指纹识别 | TideFinger潮汐   | http://finger.tidesec.net/                 |
| 指纹识别 | 云悉指纹         | https://www.yunsee.cn/                     |
| 指纹识别 | WhatWeb          | https://github.com/urbanadventurer/WhatWeb |
| 指纹识别 | 数字观星Finger-P | https://fp.shuziguanxing.com/#/            |

\#后端-闭源-配置不当-源码泄漏

参考：https://www.secpulse.com/archives/124398.html

备份：敏感目录文件扫描

CVS：https://github.com/kost/dvcs-ripper

GIT：https://github.com/lijiejie/GitHack

SVN：https://github.com/callmefeifei/SvnHack

DS_Store：https://github.com/lijiejie/ds_store_exp

 

源码泄漏原因：

1、从源码本身的特性入口

2、从管理员不好的习惯入口

3、从管理员不好的配置入口

4、从管理员不好的意识入口

5、从管理员资源信息搜集入口

源码泄漏集合：

composer.json

git源码泄露

svn源码泄露

hg源码泄漏

网站备份压缩文件

WEB-INF/web.xml 泄露

DS_Store 文件泄露

SWP 文件泄露

CVS泄露

Bzr泄露

GitHub源码泄漏

 

\#后端-方向-资源GITHUB-源码泄漏

解决1：识别出大致信息却无下载资源

解决2：未识别出信息使用码云资源获取

解决3：其他行业开发使用对口资源站获取

涉及：

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