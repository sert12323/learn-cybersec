# JS应用&WebPack打包器&第三方库JQuery&安装使用&安全检测

\#打包器-WebPack-使用&安全

参考：https://mp.weixin.qq.com/s/J3bpy-SsCnQ1lBov1L98WA

Webpack是一个模块打包器。在Webpack中会将前端的所有资源文件都作为模块处理。它将根据模块的依赖关系进行分析，生成对应的资源。

 

五个核心概念:

1. 【入口(entry)】：指示webpack应该使用哪个模块，来作为构建内部依赖图开始。
2. 【输出(output)】：在哪里输出文件，以及如何命名这些文件。
3. 【Loader】：处理那些非JavaScript文件（webpack 自身只能解析 JavaScript和json）。webpack 本身只能处理JS、JSON模块，如果要加载其他类型的文件(模块)，就需要使用对应的loader。
4. 【插件(plugins)】：执行范围更广的任务，从打包到优化都可以实现。
5. 【模式(mode)】：有生产模式production和开发模式development。

 

使用：

1、创建需打包文件

2、安装webpack库

3、创建webpack配置文件

4、运行webpack打包命令

安全：

1、WebPack源码泄漏-模式选择

2、模糊提取安全检查-PacketFuzzer

https://github.com/rtcatc/Packer-Fuzzer

原生态JS：前端语言直接浏览器显示源代码

NodeJS：服务段语言浏览器不显示源代码

WebPack：打包模式选择开发者模式后会造成源码泄漏（nodejs vue）

 

\#第三方库-JQuery-使用&安全

jQuery是一个快速、简洁的JavaScript框架,是一个丰富的JavaScript代码库。设计目的是为了写更少的代码，做更多的事情。它封装JavaScript常用功能代码，提供一种简便的JavaScript设计模式，优化HTML文档操作、事件处理、动画设计和Ajax交互。

 

1、使用：

引用路径：https://www.jq22.com/jquery-info122

2、安全：

检测：http://research.insecurelabs.org/jquery/test/

测试：CVE-2020-11022/CVE-2020-11023

参考：https://blog.csdn.net/weixin_44309905/article/details/120902867