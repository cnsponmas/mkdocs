# 手机网络游戏SDK集成指南

BSGameSdk开发包（简称：SDK）主要用来向第三方应用程序提供便捷、安全以及可靠的登录、支付服务。本文主要描述SDK用户注册、登录、修改密码、修改账号以及支付接口的使用方法，供合作伙伴的开发者接入使用。

## 1、SDK接入流程

### 1.1、接入前期准备

接入前期准备工作包括签约和密钥配置，已完成商户可略过。

需要的参数包括：

| 参数名称        | 参数说明             |
| ----------- | :--------------- |
| server_id   | 我方分配的服务器id，可能有多个 |
| merchant_id | 我方分配的商户id        |
| app_id      | 我方分配的游戏id        |
| app_key     | 游戏客户端密钥          |
| secret_key  | 游戏服务端密钥          |

- 这些参数请联系运营人员为您配置 

> app_key是客户端签名所使用的key,在sdk初始化的时候必须参数     
> secret_key请妥善保存，**为了防止泄露，不能出现在手机客户端** 



### 1.2、SDK接入流程

#### 1、资源导入

SDK解压，在Xcode中选择“Add files to 'Your project name'…”，将解压后的'接入内容'下的子文件夹（包含BlGameSdkLib下的静态库和头文件，BLGameSdkRes下的资源文件）添加到你的工程目录中。

![xcode导入](../images/ios/xcode_import.png)



导入文件后，检查`Target -Build Phases -> Compile Sources`

![xcode导入](../images/ios/xcode_setting4.png)



检查`Target - > Build Phases->Copy Bundle Resources`

![xcode导入](../images/ios/xcode_setting5.png)




#### 2、添加Framework

在 XCODE->TARGETS->Build Phases->Link Binary With Libraies 添加Framwork。

>- CoreData.framework
>- Security.framework
>- AdSupport.framework
>- StoreKit.framework
>- SystemConfiguration.framework
>- CoreGraphics.framework
>- UiKit.framework
>- Foundation.framework 

#### 3、Build Setting配置

##### 3.1、 设置 Linker flags，检查 library path

在Targets->Build Settings->Linking->Other linker Flags里添加`-ObjC`和`-lresolv`
![xcode导入](../images/ios/xcode_setting1.png)



检查library Search Paths是否自动添加lib的路径

![xcode导入](../images/ios/xcode_setting2.png)



##### 3.2、 设置 enable bitcode项目

在 Targets->Build Settings->Build Options->Enable Bitcode 里设置`No`

![xcode导入](../images/ios/xcode_setting3.png)



#### 4、plist配置

##### 4.1、在项目的plist文件中配置` LSApplicationQueriesSchemes`  并添加` bilibili.oauth.v2`

![xcode导入](../images/ios/plist_setting1.png)



也可以

```objective-c
<key>LSApplicationQueriesSchemes</key>
	<array>
		<string>bilibili.oauth.v2</string>
	</array>
```



##### 4.2、 允许非HTTPS 

在项目的plist文件中添加 `App Transport Security Settings` 的配置,并设置 `Allow Arbitrary Loads`为`YES`

![xcode导入](../images/ios/plist_setting2.png)



也可以

```objective-c
<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>
```



## 2、规范

### 2.1、包名规范

游戏包名定义加上“.bili”后缀, 如原游戏bundle为`com.example.ap`，则修改为`com.example.ap.bili`，也可以使用`com.bilibili.example.ap`

### 2.2、闪屏和Logo

闪屏页面需加入bilibili字样

![闪屏](../images/ios/bili.png)





icon需要添加角标

![Logo](../images/ios/icon.png)





## 3、接口说明

### 3.1、 Protocol & delegate

BLGameSdkDelegate用来回调SDK的登陆以及获取用户信息的结果,请实现下列协议。

```objective-c
@required
- (void)didLoginSuccessWithAccessKey:(NSString *)accessKey uid:(NSString *)uid;
- (void)didLoginFailureWithError:(NSError *)error;
- (void)didPaySuccessWithPayment:(BLPayment *)payment;
- (void)didPayFailureWithPayment:(BLPayment *)payment error:(NSError *)error;
- (void)didLogout;

@optional
- (void)didGetUserInfoSuccessWithBLAuth:(BLAuth *)auth;
- (void)didGetUserInfoFailureWithError:(NSError *)error;
```

分别是用来登录、支付、获取用户信息的回调信息。

**didLogout是用来通知游戏账号失效或登出的接口,必须接入。**

**Notice:**

**支付失败的返回值中商品消费单号可能为空，此时说明下单失败。**

### 3.2、 初始化sdk

请在调用其他接口前先初始化SDK，请勿重复调用初始化接口。

```objective-c
/**
 *  gameid 即 appid,游戏id
 *  cpid 即 merchantId,商户id
 *  serverid , 我方分配的serverid，如果有多个，初始化的时候可填其中任意一个
 *  appkey 客户端token
 *  sandboxkey 沙盒支付验证key
 *  delegate sdk委托，用于接收回调信息
 */
- (void)initWithGameid:(NSString *)gameid
                  cpId:(NSString *)cpid
              serverid:(NSString *)serverid
                appKey:(NSString *)appKey
            sandboxKey:(NSString *)key
              delegate:(id<BLGameSdkDelegate>) delegate;
```

参数说明：

| 参数         | 说明                                    |
| ---------- | :------------------------------------ |
| gameid     | 我方分配给CP对应游戏的id，即appid                 |
| cpid       | 我方分配给CP的id, 即merchantid               |
| serverid   | **我方**分配给CP对应游戏的服务器id，如果有多个可以使用其中任意一个 |
| appkey     | 我方分配给CP的**客户端**的密钥，请勿使用服务器端密钥         |
| sandboxkey | 沙盒支付验证的密钥，双方约定生成，每次版本都需要修改            |
| delegate   | 实现的用来接受回调信息的委托                        |

返回值：无

示例：

```objective-c
[[BLGameSdk defaultGameSdk] initWithGameid:@"85"
                                          cpId:@"2"
                                      serverid:@"159"
                                        appKey:@"bcf9f03f94234804a2aa11f6c9f4ccf0"
                                    sandboxKey:@"abc123"
                                      delegate:self];
```

### 3.3、 登录接口

```objective-c
/**
 *  登陆接口
 */
- (void)showLoginView;
```

参数：无

登录成功回调返回：

| 返回值       | 说明                |
| --------- | :---------------- |
| uid       | 用户唯一标识ID          |
| accessKey | 用户登陆的access_token |

登录失败回调返回：

| 返回值     | 说明   |
| ------- | :--- |
| NSError | 失败信息 |

**登陆成功后需要向服务器验证登陆，具体参考服务器API文档。**

**强更和维护的公告，必须在SDK调用登录之前显示**

**登录成功后需要调用一次notifyZone接口。（新用户还未创建角色时除外）**

示例：

```objective-c
[[BLGameSdk defaultGameSdk] showLoginView];
```

### 3.4、 获取用户信息

```objective-c
/**
 *  获取用户信息接口
 */
- (void)getUserInfoWithAccessKey:(NSString *)access;
```

参数说明：

| 参数        | 说明                              |
| --------- | ------------------------------- |
| delegate  | 实现BLGameSdkDelegate的对象,用来回调登陆结果 |
| accessKey | 登陆成功后获得的token                   |

成功回调返回：`BLAuth`对象
**Notice: account,expire的返回值可能为空**

```objective-c
@property(nonatomic,strong) NSString *mid;          // 用户唯一ID
@property(nonatomic,strong) NSString *accessKey;    // 登录凭证
@property(nonatomic,strong) NSString *account;      // 用户名
@property(nonatomic,strong) NSString *nickname;     // 用户昵称
@property(nonatomic,strong) NSString *avatar;       // 头像地址
@property(nonatomic,strong) NSString *big_avatar;   // 大头像地址
@property(nonatomic,strong,readonly) NSString *expire;       // 过期时间
@property(nonatomic,assign) BOOL     activated;     // 是否激活
```

失败回调返回：

| 返回值     | 说明   |
| ------- | :--- |
| NSError | 失败信息 |

示例：

```objective-c
[[BLGameSdk defaultGameSdk] getUserInfoWithAccessKey:accessKey];	
```

### 3.5、通知用户区服信息接口

**请在用户进入游戏，选择区服角色后调用此接口（只调用一次）**

```objective-c
/**
 *  通知区服用户信息接口
 *  serverid, 我放分配的服务器id, 请确保是当前用户所在区服
 *  serverName , 服务器名
 *  roleid,  游戏角色id
 *  roleName, 游戏角色名
 */
- (void)notifyZoneWithServerid:(NSString *)serverid
                    serverName:(NSString *)servername
                        roleid:(NSString *)roleid
                      roleName:(NSString *)roleName;
```

参数说明：

| 参数         | 说明                           |
| ---------- | ---------------------------- |
| serverid   | **我方分配的服务器id**, 请确保是当前用户所在区服 |
| serverName | 区服名称                         |
| roleid     | 游戏角色id                       |
| roleName   | 游戏角色名                        |

返回值：无

Log中若有日志如下，说明通知区服成功：

```objective-c
notifyZone result:0
```

示例：

```objective-c
 [[BLGameSdk defaultGameSdk] notifyZoneWithServerid:@"159"
                                          serverName:@"bilibili 1区"
                                              roleid:@"10086"
                                            roleName:@"22娘"];
```

### 3.6、登出

```objective-c
/**
 *  登出
 */
- (void)logout;
```

参数：无

返回值：无

示例：

```objective-c
[[BLGameSdk defaultGameSdk] logout];
```

### 3.7、支付

```objective-c
/**
 *  内购支付
 *  identifier, 商品内购identifier
 *  outTradeNo, 商户的订单号
 *  money, 商品价格， 如6rmb 则为 600， 单位为分
 *  productName, 商品名称， 如 ”钻石“
 *  productCount, 商品数量， 如 “100”
 *  extension, 透传信息
 *  accessKey, 登陆后的token
 *  delegate, 支付委托
 */
- (void)payWithItemIdentifier:(NSString *)identifier
                   outTradeNo:(NSString *)outTradeNo
                        money:(NSUInteger)money
                  productName:(NSString *)productName
                 productCount:(NSUInteger)count
           productDescription:(NSString *)des
                    extension:(NSString *)extensionInfo
                    accessKey:(NSString *)accessKey;
```

参数说明：

| 参数                 | 说明                                       |
| ------------------ | ---------------------------------------- |
| identifier         | 商品id（苹果后台配置的商品id）                        |
| outTradeNo         | **CP方订单号**（由CP服务器生成）                     |
| money              | 商品价格（**人民币，单位分**，1 rmb就是 100）            |
| productName        | 商品名称, 如“钻石”                              |
| productCount       | 购买此商品获得的游戏币数量（例如:商品名称是:100钻石礼包，那么该参数传入100） |
| productDescription | 商品描述， 如 “100钻石”                          |
| extension          | 透传信息，会在支付成功后服务器回调返回                      |
| accessKey          | 登陆后获得的access token                       |

返回值：

会通过`BLGameSdkDelegate`来回调支付有结果

成功返回：`BLPayment `对象

```objective-c
@property (nonatomic, strong) NSString *identifier;         // 商品id, 内购产品的产品id
@property (nonatomic, assign) NSUInteger money;             // 商品价格, 例如 600 , 单位是分
@property (nonatomic, strong) NSString *outTradeNo;         // 商户订单号
@property (nonatomic, strong) NSString *productName;        // 商品名称, 如“钻石”
@property (nonatomic, assign) NSUInteger productCount;      // 商品数量, 如“100”
@property (nonatomic, strong) NSString *prodectDescription; // 商品描述, 如“100钻石”
@property (nonatomic, strong) NSString *extension;          // 透传信息
@property (nonatomic, assign) NSUInteger quantity;          // 购买数量, 默认是1
@property (nonatomic, strong) NSString *blRechargeOrderNo;  // Bilibili充值订单号
@property (nonatomic, strong) NSString *blOrderNo;          // Bilibili消费订单号， 用于查单
@property (nonatomic, assign) NSInteger guess;              // 是否猜单
```

其中**消费订单号**可用于查单，查单接口请参考服务器端文档。

失败返回：

| 返回值     | 说明   |
| ------- | :--- |
| NSError | 失败信息 |

**特别注意：支付结果以服务器回调为准，SDK回调结果仅供参考**

（SDK回调支付失败，但是SDK服务器是支付成功的）例如iOS延迟回调

（例如：CP客户端收到SDK客户端的回调是【支付失败】，但是稍后CP服务器收到SDK服务器的异步通知结果是【支付成功】，那么当以CP服务器收到的为准。）

示例：

```objective-c
NSString *outTradeNo = [NSString stringWithFormat:@"test-demo-%f", [NSDate timeIntervalSinceReferenceDate]];
    [[BLGameSdk defaultGameSdk] payWithItemIdentifier:@"com.blgamesdk.demo.item1"
                                           outTradeNo:outTradeNo
                                                money:600
                                          productName:@"钻石"
                                         productCount:100
                                   productDescription:@"100钻石"
                                            extension:outTradeNo
                                            accessKey:accessKey];
```

### 3.8、 创建角色（必接）

需要在创建角色的时候调用此接口**（每次游戏创建角色的时候只能调用一次）**

**创建角色成功后，需要顺序调用`createRole`和`notifyZone`接口**

```objective-c
/**
 *  创建角色
 *  role,  游戏角色名
 *  roleid, 游戏角色id
 */
- (void)createRole:(NSString *)role roleid:(NSString *)roleid;
```

参数说明：

| 参数     | 说明     |
| ------ | ------ |
| role   | 游戏角色名  |
| roleid | 游戏角色id |

返回值：无

示例：

```objective-c
[[BLGameSdk defaultGameSdk] createRole:@"22娘" roleid:@"10086"];
```

### 3.9、 实名认证信息

需要在获取用户实名认证信息的时候调用此接口

```objective-c
/**
 实名认证信息接口

 @return 是否实名认证
 */
- (BOOL)isRealNameAuth;
```

参数：无

返回值：（当前已登录状态）                

| 返回值  | 说明    |
| ---- | ----- |
| YES  | 已实名认证 |
| NO   | 未实名认证 |

示例：

```objective-c
 BOOL isRealName = [[BLGameSdk defaultGameSdk] isRealNameAuth];
```



## 4、一键登录配置

### 4.1、 设置游戏`open url`

1、 在 info->URL Types 中添加 `url scheme`（注意保证唯一，避免重复）

![Logo](../images/ios/openurl.png)



2、调用SDK设置`openUrl`地址如下，格式为`XXX://passToken/`，XXX为游戏url

```objective-c
NSString *openUrl = @"biligame://passToken/";
[[BLGameSdk defaultGameSdk] setGameOpenUrl:openUrl];
```

3、在程序`AppDelegate.m`中添加相关方法

```objective-c
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation{
    [[BLGameSdk defaultGameSdk] didGetToken:url];
    return YES;
}

- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{
    [[BLGameSdk defaultGameSdk] didGetToken:url];
    return YES;
}
```

4、在程序plist中添加相关配置

![Logo](../images/ios/plist_setting3.png)



也可以：

```objective-c
<key>CFBundleURLTypes</key>
	<array>
		<dict>
			<key>CFBundleTypeRole</key>
			<string>Editor</string>
			<key>CFBundleURLSchemes</key>
			<array>
				<string>biligame</string>
			</array>
		</dict>
	</array>
```

### 4.2、 设置游戏icon url （不用接）

设置游戏icon的url地址。具体可自行配置，或向我方咨询



## 5、 更换皮肤**（已废弃）**

如果需要定制自己登陆界面，可使用下面的方法

### 5.1、 设置皮肤字体接口

```objective-c
/**
 *  设置主题（Option）
 */
- (void)setThemeBundleName:(NSString *)bundleName;

- (void)setThemeFontColor:(UIColor *)color;

- (void)setButtonEdgeInsets:(UIEdgeInsets)edgeInsets;

- (void)setBackgroundEdgeInsets:(UIEdgeInsets)edgeInsets;

- (void)setConfirmButtonFontColor:(UIColor *)color;

- (void)setRegButtonFontColor:(UIColor *)color;

- (void)setCheckBoxFontColor:(UIColor *)color;
```

| 接口                     | 说明               |
| ---------------------- | ---------------- |
| ThemeFontColor         | 设置主题字体颜色         |
| ButtonEdgeInsets       | 设置按钮背景图片的resize  |
| BackgroundEdgeInsets   | 设置SDK界面背景的resize |
| ConfirmButtonFontColor | 设置确认按钮的字体颜色      |
| RegButtonFontColor     | 设置注册按钮的字体颜色      |
| CheckBoxFontColor      | 设置单选框的字体颜色       |

### 5.2、 提供新的皮肤包

如果需要使用自己的皮肤，请对应default.bundle下的图片，提供一套自己的图片

![bundle](../images/ios/ios_bundle.png)





## 6、 cocos2d & Unity需读

### 6.1、 ARC设置

请检查工程是否是ARC

![arc](../images/ios/arc.png)



如果不是需要对**我方所有代码文件**添加`-fobjc-arc`的编译选项，如下

![arc](../images/ios/setting_arc.png)



###6.2、 cocos2d游戏在iOS7上登录时奔溃 

如果在iOS7的设备上出现`Assertion error in –[CCEAGLView layoutSublayersOfLayer:]`的错误，则修改
`CCEAGLView-ios.mm`的`layoutSubviews`的方法

添加如下的方法

![arc](../images/ios/ios_cocos2d.png)



# 7、 错误码

| 状态码    | 描述                  |
| ------ | ------------------- |
| 500018 | 下单失败                |
| 600000 | 服务器配置错误             |
| -15    | 游客无法支付              |
| -1     | AppKey不存在或者已封禁      |
| -3     | 无效的API签名(程序错误)      |
| -500   | 服务器内部错误             |
| -502   | 服务器内部API请求错误        |
| 90002  | 升级成功但支付失败，用户请重新登录游戏 |
| 1      | 用户取消支付              |
| 80001  | 请检查当前网络,无效的json     |
| -3     | 本地请求校验接口超时          |
| -2     | 下单失败                |
| -1     | 查询商品                |
| 0      | 下单成功, 还未支付          |
| 1      | 支付成功，充值失败           |
| 2      | 支付成功且验证成功           |
| 3      | 支付成功，消费失败           |
| 4      | 支付成功，验证失败， 需重试      |
| 5      | 取消支付                |
| 6      | 支付成功，通知苹果ing        |
| 7      | 支付验证失败，in_app为空     |
| 8      | 服务端支付验证超时           |
| 90001  | 没有此商品，或者网络错误        |







