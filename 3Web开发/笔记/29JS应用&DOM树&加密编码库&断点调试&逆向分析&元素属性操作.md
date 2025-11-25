# 29JS应用&DOM树&加密编码库&断点调试&逆向分析&元素属性操作

\#JS原生开发-DOM树-用户交互

DOM：文档操作对象

浏览器提供的一套专门用来操作网页代码内容的功能，实现自主或用户交互动作反馈

 

安全问题：本身的前端代码通过DOM技术实现代码的更新修改，但是更新修改如果修改的数据可以由用户来指定，就会造成DOM-XSS攻击！

1、获取对象

标签：直接写

Class：加上符号.

id：加上符号#

<h1 id="myHeader" onclick="getValue()">这是标题</h1>

document.querySelector('h1')

document.querySelector('.id')

document.querySelector('#myHeader')

 

2、获取对象属性

<h1 id="myHeader" onclick="getValue()">这是标题</h1>

const h1=document.querySelector('h1')

const id=h1.id

console.log(id)

 

2、操作元素数据

innerHTML 解析后续代码

innerText 不解析后续代码

 

3、操作元素属性

className src id等

<img src="iphone.jpg" width="300" height="300"></img>

const src=document.querySelector('img')

src.src='huawei.png'

 

\#JS导入库开发-编码加密-逆向调试

//Base64

 

//MD5

<script src="js/md5.js"></script>

<script>

  var str1 = 'xiaodisec'

  var str_encode = md5(str1);

  console.log(str_encode) 

</script>

 

//SHA1

<!DOCTYPE html>

    <script src="js/crypto-js.js"></script>

    <script>

​    var str1 = 'xiaodisec';

​    var str_encode = CryptoJS.SHA1(str1).toString(); // 注意：1是数字1

​    console.log(str_encode)  

  </script>

</html>    

 

//HMAC

<script src="js/crypto-js.js"></script>

<script>

  var key = 'key';

  var str1 = 'xiaodisec';

  var hash = CryptoJS.HmacSHA256(key, str1);

  var str_encode = CryptoJS.enc.Hex.stringify(hash);

  console.log(str_encode)  // '11a7960cd583ee2c3f1ed910dbc3b6c3991207cbc527d122f69e84d13cc5ce5c'

</script>

 

 

//AES

<script src="js/crypto-js.js"></script>

    <script type="text/javascript">

​     var aseKey = "12345678"   // 定制秘钥，长度必须为：8/16/32位, 长度不一致也没问题

​     var message = "xiaodisec";  // 需要加密的内容

​     // 加密 DES/AES切换只需要修改 CryptoJS.AES <=> CryptoJS.DES

​     var encrypt = CryptoJS.AES.encrypt(message, CryptoJS.enc.Utf8.parse(aseKey),  // 参数1=密钥, 参数2=加密内容

​       {

​        mode: CryptoJS.mode.ECB, // 为DES的工作方式

​        padding: CryptoJS.pad.Pkcs7  // 当加密后密文长度达不到指定整数倍(8个字节、16个字节)则填充对应字符

​       }

​     ).toString(); // toString=转字符串类型

 

​     console.log(encrypt); // 在弹窗中打印字符串 2vcsEDJv9vAZZLgFLjkZ9A==

 

​     //解密

​     var decrypt = CryptoJS.AES.decrypt(encrypt, CryptoJS.enc.Utf8.parse(aseKey), // 参数1=密钥, 参数2=解密内容

​       {

​        mode: CryptoJS.mode.ECB,

​        padding: CryptoJS.pad.Pkcs7

​       }

​     ).toString(CryptoJS.enc.Utf8); // toString=转字符串类型,并指定编码

​     console.log(decrypt); // "xiaodisec"

  </script>

 

//DES

<script src="js/crypto-js.js"></script>

<script type="text/javascript">

   var aseKey = "12345678"   // 定制秘钥，长度必须为：8/16/32位， 长度不一致也没问题

   var message = "xiaodisec";  // 需要加密的内容

   // 加密 DES/AES切换只需要修改 CryptoJS.AES <=> CryptoJS.DES

   var encrypt = CryptoJS.DES.encrypt(message, CryptoJS.enc.Utf8.parse(aseKey),  // 参数1=密钥, 参数2=加密内容

​     {

​      mode: CryptoJS.mode.ECB, // 为DES的工作方式

​      padding: CryptoJS.pad.Pkcs7  // 当加密后密文长度达不到指定整数倍(8个字节、16个字节)则填充对应字符

​     }

   ).toString(); // toString=转字符串类型

 

   console.log(encrypt); // 控制台打印 CDVNwmEwDRM

 

   //解密

   var decrypt = CryptoJS.DES.decrypt(encrypt, CryptoJS.enc.Utf8.parse(aseKey), // 参数1=密钥, 参数2=解密内容

​     {

​      mode: CryptoJS.mode.ECB,

​      padding: CryptoJS.pad.Pkcs7

​     }

   ).toString(CryptoJS.enc.Utf8); // toString=转字符串类型,并指定编码

   console.log(decrypt); // 控制台打印 "i am xiaozhou ?"

</script>

 

 

//RSA

<script src="js/jsencrypt.js"></script>

    <script type="text/javascript">

​    // 公钥 私匙是通过公匙计算生成的，不能盲目设置

​    var PUBLIC_KEY = '-----BEGIN PUBLIC KEY-----MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBALyBJ6kZ/VFJYTV3vOC07jqWIqgyvHulv6us/8wzlSBqQ2+eOTX7s5zKfXY40yZWDoCaIGk+tP/sc0D6dQzjaxECAwEAAQ==-----END PUBLIC KEY-----';

​    //私钥

​    var PRIVATE_KEY = '-----BEGIN PRIVATE KEY-----MIIBVQIBADANBgkqhkiG9w0BAQEFAASCAT8wggE7AgEAAkEAvIEnqRn9UUlhNXe84LTuOpYiqDK8e6W/q6z/zDOVIGpDb545NfuznMp9djjTJlYOgJogaT60/+xzQPp1DONrEQIDAQABAkEAu7DFsqQEDDnKJpiwYfUE9ySiIWNTNLJWZDN/Bu2dYIV4DO2A5aHZfMe48rga5BkoWq2LALlY3tqsOFTe3M6yoQIhAOSfSAU3H6jIOnlEiZabUrVGqiFLCb5Ut3Jz9NN+5p59AiEA0xQDMrxWBBJ9BYq6RRY4pXwa/MthX/8Hy+3GnvNw/yUCIG/3Ee578KVYakq5pih8KSVeVjO37C2qj60d3Ok3XPqBAiEAqGPvxTsAuBDz0kcBIPqASGzArumljkrLsoHHkakOfU0CIDuhxKQwHlXFDO79ppYAPcVO3bph672qGD84YUaHF+pQ-----END PRIVATE KEY-----';

​    //使用公钥加密

​    var encrypt = new JSEncrypt();//实例化加密对象

​    encrypt.setPublicKey(PUBLIC_KEY);//设置公钥

​    var message = 'xiaodisec' // 需要加密的数据 

​    var encrypted = encrypt.encrypt(message);//对指定数据进行加密

​    console.log(encrypted)  // 'JQ83h8tmJpsSZcb4BJ3eQvuqIAs3ejepcUUnoFhQEvum8fA8bf1Y/fG+DO1bSIVNJF6EOZKe4wa0njv6aOar9w=='

​    //使用私钥解密

​    var decrypt = new JSEncrypt(); // 创建解密对象

​    decrypt.setPrivateKey(PRIVATE_KEY); //设置私钥

​    var uncrypted = decrypt.decrypt(encrypted); //解密 'xiaodisec'

​    console.log(uncrypted);

  </script>