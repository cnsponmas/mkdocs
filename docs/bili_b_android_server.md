## 1、概述
本文档是哔哩哔哩游戏SDK服务端API的统一接口规范和开发指南。主要提供给哔哩哔哩游戏中心的“SDK服务器”和哔哩哔哩游戏合作商的“游戏服务器”的交互接口规范并且配有java版和php版的相关demo，请向商务人员索取  

# 2、协议说明
##  2.1、通信协议
**(1)概述**  
本接口采用HTTP 协议作为通信协议，调用方通过构造HTTP 请求（POST/GET 方式）向““哔哩哔哩游戏SDK服务器”发起接口请求。  
本服务为游戏服务器调用哔哩哔哩游戏SDK服务端请求数据接口，由于南北网络连通性问题，我们提供了多域名解决方案，游戏研发需要有切线机制，主线不通切到备线，接口域名如下：  
http://pnew.biligame.net（主线）  
http://line3-qcloud-game-api-adapter-na.biligame.net（备用）
http://pserver.bilibiligame.net（备用）  


**(2)游戏服务端请求数据示例**  
>采用POST请求，请求body为URL参数，示例如下：  
```
POST http://pnew.biligame.net/api/server/session.verify
sign=be61b558417cd078bfe514788999ebd8&timestamp=1418119041368&server_id=2&game_id=2&access_key=7d4c3cacccf28a7e342e4587a17139c7&merchant_id=2&uid=123&version=1
```
**(3)哔哩哔哩游戏SDK服务端请求数据示例**  
```
POST http://game-server.test.com/recharge/callback
Content-Type: text/html
success
```
**(4)响应数据示例**
>以下为正常返回数据  
```
200 OK
Content-Type: application/json
{
"timestamp":1418119041368,
    	"code":0,
    	"open_id":123,
"uname":"xxxxxx"
}
```
>以下为异常返回数据
```
200 OK
Content-Type: application/json
{
    "code": -626,
    "message": "API sign invalid"
}
```
## 2.2、数据协议
### 2.2.1、数据格式
**(1)游戏服务端请求**  
>请求头部信息  

参数 | 是否必填 | 头部值 | 描述
-----|-----|-----|---
User-Agent | Y | Mozilla/5.0 GameServer | 常量，所有API都是这个值

>请求消息公共字段(公共请求参数)

参数 | 是否必填 | 类型 | 描述
-----|-----|-----|------
game_id|Y|int|游戏id，即为CP分配的game_id
merchant_id|Y|int|商务id，即为CP分配的merchant_id
server_id|N|int|游戏区服ID
uid|Y|int|B站用户uid
version|Y|String|默认为1
timestamp|Y|int|当前时间戳(毫秒)
sign|Y|String|签名

>响应消息公共字段如下表格  

参数 | 是否必填 | 类型 | 描述
-----|-----|-----|------
timestamp|Y|int|时间戳(秒)，对应request的
code|Y|int|状态码
message|Y|String|错误信息，code不为0的时候出现

>响应消息头部信息
包含Connection，Content-Type，Server，Date等标准参数

参数 | 是否必填 | 头部值 | 描述
-----|-----|-----|------
Connection|Y|keep-alive|
Content-Type|Y|application/json; charset=utf-8
Server|Y|nginx
Date|Y|Thu, 23 Mar 2017 08:48:42 GMT

**(2)哔哩哔哩游戏SDK服务端请求**  
>请求消息字段

参数 | 是否必填 | 类型 | 描述
-----|-----|-----|------
data|Y|json|请求的json数据

### 2.2.2、字符编码
>请求与响应内容须采用UTF-8字符编码。


### 2.2.3、签名方法
**(1)游戏服务端请求**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;游戏服务端请求，请求方为游戏服务端，请求数据为URL参数方式，如：用户会话验证接口。把接口所需所有URL参数，按参数名称排序，排除**item_name**以及**item_desc**字段，将其他参数值拼接，如utk=aa&time=bb，拼接后为bbaa，最后再拼接上商务所提供约定的密钥secret_key=cc，
则最后拼接出来的字符串为bbaacc，对此字符串进行md5加密  
>字段：拼接时需对字段名排序，排序方式是按字段名进行字符串升序排列。  
>字段值：取值只能为字符串、数字、true、false、null五类（注意true、false、null为小写，非True、False、Null等）。   
>**计算MD5签名时，取签名内容的字节时，应以UTF-8编码取字符串的字节值。**  

**(2)哔哩哔哩游戏SDK服务端请求**  
>SDK服务端请求，请求方为哔哩哔哩游戏SDK服务端，请求数据为URL参数方，参数只有data，data的类型为，JSON格式，如：充值结果回调接口。  

>请求data样例：  
```
{
    "id": "255",
    "order_no": "2014031010000614",
    "out_trade_no": "188292BFE31121A83ACC84909718EF61",
    "uid": "3521571",
    "username": "brianyao2014",
    "role": "android",
    "money": "1000",
    "pay_money": "1000",
    "game_money": "10000",
    "merchant_id": "5",
    "game_id": "9",
    "zone_id": "9",
    "product_name": "蓝钻",
    "product_desc": "Diamond",
    "pay_time": "1394434881",
    "client_ip": "221.223.236.205",
    "extension_info": "543002:android:3521571",
    "order_status": 1,
    "sign": "8f7160f8bc8a262660f2c7a42afabdb1"
}
```
>Json decode后得到数组：
```
Array
(
[id] => 255
[order_no] => 2014031010000614
[out_trade_no] => 188292BFE31121A83ACC84909718EF61
[uid]=> 3521571
[username] => brianyao2014
[role] => android
[money] => 1000
[pay_money] => 1000
[game_money] => 10000
[merchant_id] => 5
[game_id] => 9
[zone_id] => 9
[product_name] =>\u84dd\u94bb
[product_desc] => Diamond
[pay_time] => 1394434881
[client_ip] => 221.223.236.205
[extension_info] => 543002:android:3521571
[order_status] => 1
   	[sign] => 8f7160f8bc8a262660f2c7a42afabdb1
)
```
商户需要对参数进行签名校验，去掉sign参数,将其他参数按照数组键值顺序升序排列,再把所有数组值连接起来，形成的字符串末尾加上商户与拼接上商务所提供约定的密钥secret_key即服务端appkey，之后整体做MD5，然后全部转成小写，将产生的加密串与sign进行对比，若相符，则该次请求为合法请求。  

**(3)订单参数加签规则**  
* CP客户端调用哔哩哔哩游戏SDK客户端创建消费订单（接口详情参见《哔哩哔哩游戏SDK-客户端接入文档.doc》第4.7节）时，其中入参order_sign（订单参数签名）值为：按顺序拼接游戏内货币、本次交易金额、支付回调地址、商户订单号、商户所提供约定的服务端密钥后，进行md5加签的值。  
* 哔哩哔哩游戏SDK服务端的创建消费订单接口接收到入参order_sign后，将校检订单信息是否被篡改。如发现订单被篡改，则拒绝下单。  
* **加签方法：**  
>对哔哩哔哩游戏SDK客户端创建消费订单接口参数game_money（游戏内货币） 、 money（本次交易金额） 、 notify_url（支付回调地址） 、 out_trade_no（商户订单号） 按顺序进行拼接，再拼接上商务所提供约定的服务端密钥secret_key，最后对此字符串进行md5加密。  
*  **下单时必须在CP服务端对订单参数加签。**

*  **注： notify_url（支付回调地址）为空或null时，值设为“”。**  

*  **示例：**
```
int game_money=1;  
int money=100;  
String notify_url=http://www.biligame.com,;  
Sting out_trade_no= 5117897656814864;  
String secret_key=cc;  
String data = game_money + money + notify_url + out_trade_no + secret_key;  
String order_sign = md5(1100http://www.biligame.com5117897656814864cc);  
```

## 2.3、通用code状态码
>通用code状态码如下:

状态码 | 说明
-----|-----
-1|app_id不存在或已被封禁
-2|access key错误
-3|API校验密匙错误
-4|请求头部的user-agent不匹配，参考：2.2.1数据格式
-101|帐号未登陆
-102|帐号被封停
-400|请求参数错误
-500|服务器内部错误

# 3、接口列表
## 3.1、用户会话验证接口
* **接口描述**：验证access_key是否为有效的登录用户会话，若有效则返回其uid和昵称
* **返回数据支持格式**：JSON
* **版本**：1
* **HTTP请求方式**：POST
* **请求方**：游戏服务器
* **响应方**：哔哩哔哩游戏SDK服务器
* **请求地址**：http://pnew.biligame.net/api/server/session.verify
>注意：“如果http://pnew.biligame.net/如果请求超时或者服务不可用时，请使用http://pserver.bilibiligame.net”替换http://pnew.biligame.net/域名后再请求此接口。  
独代游戏服务端验证必须做切线处理
* **请求参数（需要传递公共请求参数,详见2.2.1数据格式）**

参数 | 必填 | 类型 | 描述
-----|-----|-----|------
access_key|Y|String|哔哩哔哩游戏SDK登录接口返回的access_key，也就是客户端登录接口返回的access_token
uid|Y|int|B站用户uid

* **特殊响应状态码说明（通用响应码见2.3节）**

状态码 | 说明 
-----|-----
暂无|暂无

* **响应数据说明（仅描述非通用字段的数据格式）**

参数 | 必填 | 类型 | 描述
-----|-----|-----|------
open_id|Y|String|哔哩哔哩游戏用户的用户id，也就是客户端登录接口返回的uid
uname|Y|String|哔哩哔哩游戏用户的昵称

* **CP方请求参数**
> 示例
```
access_key=4ac2cceb5bb64906535398c58a981a02&game_id=57&merchant_id=1&server_id=116&version=1&timestamp=1445270401897&sign=fe222a9dc8392932404485c0ce5707bd
```
* **返回结果**
>JSON示例
```
{
"code":0,
"timestamp":141645290000,
"open_id":"141642321"
"uname": "用户昵称"
}
```

## 3.2、查询支付订单接口
* **接口描述**：根据CP订单号，查询支付订单结果信息
>PS：建议CP多用同步通知机制，然后主动来查询订单结果

* **返回数据支持格式**：JSON
* **版本**：1
* **HTTP请求方式**：POST
* **请求方**：游戏服务器
* **响应方**：哔哩哔哩游戏SDK服务器
* **请求地址**：http://pnew.biligame.net/api/server/query.pay.order
>注意：“如果http://pnew.biligame.net/如果请求超时或者服务不可用时，请使用http://pserver.bilibiligame.net”替换http://pnew.biligame.net/域名后再请求此接口。  
独代游戏服务端验证必须做切线处理

* **请求参数（需要传递公共请求参数,详见2.2.1数据格式）**

参数 | 必填 | 类型 | 描述
-----|-----|-----|------
order_no|Y|String|哔哩哔哩提供的订单号
uid|Y|int|B站用户uid

* **特殊响应状态码说明（通用响应码见2.3.节）**

参数 | 必填 
-----|-----
暂无|暂无

* **响应数据说明（仅描述非通用字段的数据格式）**

参数 | 必填 | 类型 | 描述
-----|-----|-----|------
uid|Y|int|用户ID
username|Y|String|付款游戏用户的交易账号
pay_time|Y|timestamp|支付订单时间，格式为时间戳
pay_money|Y|int|支付金额，单位为分，如1分钱
order_no|Y|String|哔哩哔哩游戏SDK服务器的订单号
out_trade_no|Y|String|游戏CP厂商支付订单号
server_id|Y|int|游戏区服iD
subject|Y|String|订单名称
remark|N|String|订单备注信息
order_status|Y|int|订单状态：1为已完成; 2为失败；3为处理中
notify_status|Y|int|异步通知状态：0：未完成；1：已经完成；2：异步通知失败
extension_info|N|String|额外信息，原样通知回来，联运商返回的拓展信息

* **CP方请求参数**
>示例
```
order_no=5121979484629435&uid=85547212&sign=bb638778c69a196b491b63b8e0acebcf&merchant_id=1&server_id=543&version=1&game_id=314&timestamp=1512197961
```
* **返回结果**
>JSON示例
```
{
"timestamp":1445270440,
"code":0,
"uid":15868578,
"username":"AnubisZ",
"pay_time":1445261585,
"pay_money":3000,
"order_no":"4456431357954"
…….
}
```

## 3.3、充值结果回调接口
* **接口描述**：即充值结果通知地址，当支付成功后，哔哩哔哩游戏SDK服务器会将支付结果在notify告诉游戏服务器,由游戏CP提供。游戏接入时，由游戏合作商提供给哔哩哔哩游戏运营人员，录入到接入系统当中
* **返回数据支持格式**：JSON
* **版本**：1
* **HTTP请求方式**：POST
* **请求方**：哔哩哔哩游戏SDK服务器
* **响应方**：游戏服务器
* **请求地址**：游戏CP提供的回调地址 (CP可以联系哔哩哔哩游戏商务)
* **请求参数**

参数 | 必填 | 类型 | 描述
-----|-----|-----|------
data|Y|json|请求的数据data信息，json格式，详细请看下面的data信息

* **data信息**

参数 | 必填 | 类型 | 描述
-----|-----|-----|------
id|Y|int|订单ID
order_no|Y|String|哔哩哔哩游戏SDK服务器方订单号
out_trade_no|Y|String|游戏CP厂商支付订单号
uid|Y|int|用户ID
username|Y|String|用户名
role|Y|String|角色名
money|Y|int|支付金额（单位：分）
pay_money|Y|int|实际支付金额，单位：分
game_money|Y|int|应用内货币
merchant_id|Y|int|商户ID
game_id|Y|int|游戏ID
zone_id|Y|int|区服ID
product_name|Y|String|商品名称
product_desc|Y|String|商品描述
pay_time|Y|int|订单支付时间
client_ip|Y|String|客户端IP
extension_info|Y|String|额外信息，原样通知回来
order_status|Y|int|订单状态：1为已完成
sign|Y|String|md5加密后的签名

* **特殊响应状态码说明（通用响应码见2.3.节）**

状态码 | 说明 
-----|-----
无

* **响应数据说明（该接口只有响应内容）**

响应内容 | 描述 
-----|-----
success或者failure|success表示处理订单成功，哔哩哔哩游戏SDK服务端方收到响应success后不会再通知给cp方.failure失败（也可返回其他错误信息，bili方收到success以外的字符串后都会多次重复通知）

* **接口备注**  
在用户支付订单完成后，哔哩哔哩游戏SDK服务器会向商户方服务器发起通知，并异步不断尝试直到获取结果。以下为异步通知接口说明：

> * 必须保证服务器异步通知页面(notify_url)上无任何字符,如空格、HTML 标签、开发系统自带抛出的异常提示信息等;  
* 哔哩哔哩游戏SDK服务器使用 POST 方式发送通知信息,只有一个参数名称为data,里面的数据是一个json字符串，json decode以后得到订单数组，因此该页面中获取参数的方式,如: request.Form("data")、$_POST['data']; 
* 程序执行完后必须打印输出“success”(不包含引号，不能加入换行符，缩进符等不可见字符)。如果商户反馈给哔哩哔哩游戏SDK服务器的字符不是 success 这 7 个字符,哔哩哔哩游戏SDK服务器会不断重发通知,直到 超过24小时22分钟。一般情况下,25 小时以内完成 8 次通知(通知的间隔频率一般是: 2m,10m,10m,1h,2h,6h,15h);  
* 程序执行完成后,该页面不能执行页面跳转。如果执行页面跳转,哔哩哔哩游戏SDK服务器会收不到 success 字符,会被哔哩哔哩游戏SDK服务器判定为该页面程序运行出现异常, 而重发处理结果通知;  
* cookies、session 等在此页面会失效,即无法获取这些数据;  
* 该方式的调试与运行必须在服务器上,即互联网上能访问;
*  **CP方不仅要对回调的签名进行验证，还需要对回调的金额进行比对，两方一致方可发货，避免用户造假篡改订单内容**

该方式的作用主要防止订单丢失,即页面跳转同步通知没有处理订单更新,它则去处理;   

* **哔哩哔哩游戏SDK服务器方请求**
>示例
```
http://www.notifyUrl.com?data={
    "client_ip": "182.48.102.6",
    "extension_info": "20015312|2|ag0002",
    "game_id": "93",
    "game_money": "30",
    "id": "4114535",
    "merchant_id": "30",
    "money": "3000",
    "order_no": "4452682411635123",
    "order_status": 1,
    "out_trade_no": "01200153121445268238110020101",
    "pay_money": "3000",
    "pay_time": "1445268273",
    "product_desc": "机动战姬钻石",
    "product_name": "300钻石",
    "role": "折木奉太郎",
    "sign": "974c948dde515d39d7d1e5b52a891778",
    "uid": "389339",
    "username": "浓眉毛の喵",
    "zone_id": "184"
}
```
*  **CP方返回结果**
>示例
```
success
```

























