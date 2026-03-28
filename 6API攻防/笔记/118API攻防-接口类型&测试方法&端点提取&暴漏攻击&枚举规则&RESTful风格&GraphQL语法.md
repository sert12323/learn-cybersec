# API攻防-接口类型&测试方法&端点提取&暴漏攻击&枚举规则&RESTful风格&GraphQL语法

```java
#API接口类型：
1、RESTful API(Representational State Transfer)：RESTful API是一种基于HTTP协议的API设计风格，它使用HTTP方法(例如：GET、POST、PUT、DELETE)来对资源进行操作并通过URL来唯一标识资源
2、SOAP API(Simple Object Access Protocol)：SOAP API是一种基于XML的通信协议，它使用SOAP消息格式进行数据交换，SOAP API通常使用WSDL(Web Services Description Language)描述接口，支持复杂的数据类型和协议扩展
3、GraphQL API(Graph Query Language)：GraphQL API是一种用于数据查询和操作的API查询语言，它允许客户端定义需要返回的数据结构，从而减少不必要的数据传输和多次请求
4、gRPC API：gRPC是一种高性能、开源的远程过程调用(RPC)框架，它支持多种编程语言并使用Protocol Buffers进行数据序列化和通信
5、WebSocket API：WebSocket API提供了一种全双工通信的机制，使得服务器和客户端可以实时地进行双向数据传输，适用于实时通信和推送场景
6、JSON-RPC API：JSON-RPC是一种轻量级的远程过程调用(RPC)协议，基于JSON格式进行数据交换，支持各种编程语言和平台
7、OAuth API：OAuth是一种开放标准的授权协议，用于用户授权第三方应用程序访问受保护的资源，OAuth API提供了一组用于身份验证和授权的接口
8、OpenAPI/Swagger API：OpenAPI(以前称为Swagger)是一种用于设计、构建和文档化API的规范和工具集。OpenAPI/Swagger API提供了一种描述API接口和操作的标准方式

#API检测流程
接口发现，遵循分类，依赖语言，V1/V2多版本等

接口发现：
JS等中提取，枚举爆破，响应提示等

Method：请求方法
 攻击方式：OPTIONS,PUT,MOVE,DELETE
 效果：上传恶意文件，修改页面等
URL：唯一资源定位符
 攻击方式：猜测，遍历，跳转
 效果：未授权访问等
Params：请求参数
 攻击方式：构造参数，修改参数，遍历，重发
 效果：爆破，越权，未授权访问，突破业务逻辑等
Authorization：认证方式
 攻击方式：身份伪造，身份篡改
 效果：越权，未授权访问等
Headers：请求消息头
 攻击方式：拦截数据包，改Hosts，改Referer，改Content-Type等
 效果：绕过身份认证，绕过Referer验证，绕过类型验证，DDOS等
Body：消息体
 攻击方式：SQL注入，XML注入，反序列化等
 效果：提权，突破业务逻辑，未授权访问等

#RESTful风格的测试：
1、API接口JS中URL泄露：
fetch(
    `${form.action}/${encodeURIComponent(username)}`,
    {
        method: 'PATCH',
        body: JSON.stringify({ 'email': email })
    }
)

2、API接口泄露参数污染：
%26a=b
#
%26field=reset_token

3、API接口利用提交方法：
POST /api/products/1/price
PATCH /api/products/1/price
Content-Type: application/json
{
	"price":1
}

4、API接口利用泄露参数：
POST /api/checkout
{    
    "chosen_discount":{                  //被选择（产品）的折扣
        "percentage":0                   //百分比
},
    "chosen_products":[                  //被选择的产品
        {
            "product_id":"1",            //产品id
            "name":
            "Lightweight \"l33t\" Leather Jacket
            ",
            "quantity":1,                //数量
            "item_price":133700          //价格
                    }
            ]


#GraphQL风格的测试：（上部分）
利用核心：除常规测试思路外，语法是重点
测试插件：BurpSuite应用市场InQL
参考：https://mp.weixin.qq.com/s/gpm8w0HHW5wNKQLq4AtGyg
参考：blog.csdn.net/qq_61812944/category_12417979.html
参考：https://graphql.cn/learn/introspection/
1、实验室：访问私人GraphQL数据
2、实验室：访问暴漏GraphQL字段
3、实验室：访问隐藏GraphQL端点

和RESTful差异：
RESTful：请求什么就换一个端点和参数
GraphQL：固定的端点里面的东西改变

解决1：目标使用GraphQL API技术判断
插件URL特征分析，提交的参数数据特征

解决2：目标使用GraphQL API初级安全测试技术
见演示

解决3：目标使用GraphQL API中高级安全测试技术（下节课）

```

