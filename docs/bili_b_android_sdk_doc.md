<h1>哔哩哔哩游戏SDK集成指南</h1>
哔哩哔哩游戏SDK开发包（简称：SDK）主要用来向第三方应用程序提供便捷、安全以及可靠的登录、支付服务。本文主要描述SDK用户注册、登录、修改密码、修改账号以及支付接口的使用方法，供合作伙伴的开发者接入使用。

## 1、SDK接入前准备
接入前期准备工作包括商户签约和密钥配置，已完成商户可略过。
需要获取的参数包括：

| 参数名称         | 参数说明             |
| ------------ | ---------------- |
| server\_id   | 我方分配的服务器id，可能有多个 |
| merchant\_id | 我方分配的商户id        |
| app\_id      | 我方分配的游戏id        |
| app\_key     | 游戏客户端密钥          |
| secret\_key  | 游戏服务端密钥          |

**这些参数请联系运营人员为您配置**  

* app_key是客户端签名所使用的key,在sdk初始化的时候必须参数       
* secret_key请妥善保存，**为了防止泄露，不能出现在手机客户端** 

##2、SDK快速接入
###2.1、基础环境
本教程适用于AndroidStudio、Eclipse开发工具，如果您没有以上开发工具，可以到官网下载。

###2.2、SDK导入
**方法1：eclipse环境下以库工程引入SDK**

首先将Demo\_Project目录中的bsgamesdk\_android\_library库工程导入eclipse，并在目标工程properties->android->library添加此library project。
![eclipse导入图](../img/eclipse_import.png) 

此部分配置可以参考demo工程
>注意，导入时需要将bsgamesdk\_android\_demo和bsgamesdk\_android\_library两个工程同时导入。

**方法2：资源导入**

将接入内容目录下libs，res里的文件拷贝到目标工程的对应目录，并在目标工程里设置引用libs里的jar文件，即完成sdk的导入。

**方法3：AndroidStudio导入**

（1）工程根目录下新建libs文件夹,将aar文件放置到libs文件夹下  
（2）在gradle配置文件中添加如下代码:

```
    repositories {
        flatDir {
            dirs 'libs'
        }
    }

    dependencies {
        compile(name: 'bsgamesdk_android_library-xxx', ext: 'aar')
    }
```

（3）根据游戏使用的架构库,选择SDK架构库,在gradle配置文件中添加如下代码:

```
    buildTypes {
        release {
            ndk {
                abiFilters "armeabi-v7a", "x86" //根据游戏需要保留架构库种类
            }
        }

        debug {
            ndk {
                abiFilters "armeabi-v7a", "x86" //根据游戏需要保留架构库种类
            }
        }
    }
```

（4）混淆规则中请加入我方提供的混淆规则，混淆文件位置在 “接入内容/android studio/proguard-rules.txt”。

###2.3、assets配置
拷贝接入内容中assets下license.txt、distributor.txt、ext.txt以及service.html到游戏工程assets下。ext.txt的内容确保为空。distributor.txt的内容确保为100，如果生成的包不需要重签名则内容确保为1.（如不清楚请与我方确认）。
>拷贝libs下的so文件时，请根据游戏具体编译情况导入。比如unity默认的FAT（ARMv7 + x86）,则复制armeabi-v7a和x86的so即可。Cocos2dx查看设置的APP_ABI如果默认不设置则拷贝armeabi或者查看编译出自己的so库有哪几种类型就拷贝对应类型的so。

###2.4、修改AndroidManifest.xml

####2.4.1、包名规范
游戏客户端的 AndroidManifest.xml 中游戏包名定义加上“.bilibili”后缀。如原游戏包名为
packge="com.公司名.游戏名"，则修改为 packge="com.公司名.游戏名.bilibili"。 
>注意：在manifest标签中不要添加android:installLocation="preferExternal"，在华为等一些设备4.0以上的系统不支持设置首选项为SD卡安装应用，使用默认值就可以了。  

####2.4.2、权限申请

**请将以下内容全部复制到AndroidManifest.xml的manifest标签下**

```
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<!--paypal start-->
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-feature android:name="android.hardware.camera" android:required="false" />
<uses-feature android:name="android.hardware.camera.autofocus" android:required="false" />
<!--paypal end-->
```

####2.4.3、在application中添加activity
**添加activity前请注意以下内容**
>1.由于加入微信支付功能，接入时，需要在自己的包名下新建wxapi文件夹，然后在此文件夹下新建WXPayEntryActivity，并且WXPayEntryActivity继承SDK中的BaseWXEntryActivity，WXPayEntryActivity不需要实现任何方法。最后在AndroidManifest.xml中声明此activity。此部分可参照demo项目

>2.PaymentActivity请保持竖屏。

**请将以下内容全部复制到AndroidManifest.xml的application标签下**

```
<activity
    android:name=".wxapi.WXPayEntryActivity"
    android:exported="true"
    android:launchMode="singleTop" />
<activity
    android:name="com.bsgamesdk.android.activity.PaymentActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:screenOrientation="portrait"
    android:theme="@style/bsgamesdk_customPaymentTheme" 
    android:launchMode="singleTop" />
<activity
    android:name="com.bsgamesdk.android.activity.BSGameSdkAgreementActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
    android:launchMode="singleTop"/>
<activity
    android:name="com.bsgamesdk.android.activity.Login_RegActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgamesdk_customDialog" 
    android:launchMode="singleTop"/>
<activity
    android:name="com.bsgamesdk.android.activity.LoadingActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgamesdk_customDialog" 
    android:launchMode="singleTop"/>
<activity 
    android:name="com.bsgamesdk.android.activity.TouristActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgamesdk_customDialog" 
    android:launchMode="singleTop"/>
<activity 
    android:name="com.bsgamesdk.android.activity.WelcomeActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgemsdk_activity_welcome_style" 
    android:launchMode="singleTop" />
<activity 
    android:name="com.bsgamesdk.android.activity.NoticeActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgamesdk_customDialog" 
    android:launchMode="singleTop"/>
<activity 
    android:name="com.bsgamesdk.android.activity.PointActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgamesdk_customDialog" 
    android:launchMode="singleTop"/>
<activity 
    android:name="com.bsgamesdk.android.activity.ExitActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgamesdk_customDialog" 
    android:launchMode="singleTop"/>
<activity 
    android:name="com.bsgamesdk.android.activity.RegisterActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgamesdk_customDialog" 
    android:launchMode="singleTop"/>
 <activity 
    android:name="com.bsgamesdk.android.activity.LicenseAgreementActivity"
    android:configChanges="keyboardHidden|navigation|orientation|screenSize"
    android:theme="@style/bsgamesdk_customDialog" 
    android:launchMode="singleTop"/>

<!-- alipay  begin -->
<activity
    android:name="com.alipay.sdk.app.H5PayActivity"
    android:configChanges="orientation|keyboardHidden|navigation"
    android:exported="false"
    android:screenOrientation="behind" />
<activity
    android:name="com.alipay.sdk.auth.AuthActivity"
    android:configChanges="orientation|keyboardHidden|navigation"
    android:exported="false"
    android:screenOrientation="behind" />
<!-- alipay  end -->
<!-- paypal  start -->
<activity android:name="com.paypal.android.sdk.payments.PaymentActivity" />
<activity android:name="com.paypal.android.sdk.payments.PaymentMethodActivity" />
<activity android:name="com.paypal.android.sdk.payments.PaymentConfirmActivity" />
<activity android:name="com.paypal.android.sdk.payments.LoginActivity" />
<activity android:name="com.paypal.android.sdk.payments.PayPalFuturePaymentActivity" />
<activity android:name="com.paypal.android.sdk.payments.FuturePaymentConsentActivity" />
<activity android:name="com.paypal.android.sdk.payments.FuturePaymentInfoActivity" />
<activity android:name="com.paypal.android.sdk.payments.PayPalProfileSharingActivity" />
<activity android:name="com.paypal.android.sdk.payments.ProfileSharingConsentActivity" />
<service android:name="com.paypal.android.sdk.payments.PayPalService" android:exported="false" />
<activity
    android:name="io.card.payment.CardIOActivity"
    android:configChanges="keyboardHidden|orientation" />
<activity android:name="io.card.payment.DataEntryActivity" />
<!-- paypal  end -->
```
如果与Demo项目中存在差异，请以Demo项目中为准

##3、SDK初始化
###3.1、SDK初始化接口
初始化接口调用方式如下

```
BSGameSdk gameSdk = BSGameSdk.initialize(boolean debug, Activity instance, String merchant_id, String app_id, String server_id, String app_key, Handler handler);
```
参数说明如下

| 参数名称         | 参数说明                                     |
| ------------ | ---------------------------------------- |
| debug        | 是否打开debug模式，正式包必须关闭，值为false              |
| instance     | Activity实例                               |
| merchant\_id | cpid商户id,由平台自动分配                         |
| app\_id      | 每款应用在平台的唯一标识，由平台分配                       |
| server\_id   | 我方分配的商户应用的服务器编号，一般用来区分区服，如果有多个区服，则填其中一个默认服务器，并在选择角色、区服后调用notifyZone接口传入正确的区服id |
| app\_key     | 商户应用的客户端密钥，请勿使用服务器端密钥                    |
| handler      | looper为main looper的Handler对象             |



#### 注意：初始化接口和登录接口不能同时调用，需要间隔一段时间



###3.2、接口调用方式

获取BSGameSdk对象后，可以调用其中接口。每个接口方法的参数中都包含CallbackListener类型的监听器，用户需实现其中的3个方法：onSuccess、onFailed和onError。当调用执行完毕返回结果时，会根据返回状态的不同执行其中相应的方法。  

**代码示例**

```
//用户注册
v.register(new CallbackListener() {
	@Override
	public void onSuccess(Bundle arg0) {
        // 此处为操作成功时执行，返回值通过Bundle传回
    	LogUtils.d("onSuccess");
    	String result = arg0.getString("key");
    }
	@Override
	public void onFailed(BSGameSdkError arg0) {
	    // 此处为操作失败时执行，返回值为BSGameSdkError类型变量，
	    //其中包含ErrorCode和ErrorMessage
	    LogUtils.d("onFailed\nErrorCode : " + arg0.getErrorCode() + "\nErrorMessage : " + arg0.getErrorMessage());
	}
    @Override
    public void onError(BSGameSdkError arg0) {
        // 此处为操作异常时执行，返回值为BSGameSdkError类型变量，
        //其中包含ErrorCode和ErrorMessage
        LogUtils.d("onError\nErrorCode : " + arg0.getErrorCode() + "\nErrorMessage : " + arg0.getErrorMessage());
    }
});
```

##4、SDK接口介绍

**接入前，强烈建议运行Demo apk，从而全面了解我方SDK。接入的具体详情可以参考Demo Project**

###4.1、登录接口

```
login(CallbackListener listener);  
```
>调用该方法，会打开哔哩哔哩游戏用户登录界面，引导用户输入用户名、密码来完成登录过程。  

**参数**：  
CallbackListener监听器类  

**返回结果**：  
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为：  

| key            | 含义     | 类型     | 样例                               |
| -------------- | ------ | ------ | -------------------------------- |
| uid            | 用户ID   | String | 10001                            |
| username       | 用户名昵称  | String | gamenick                         |
| access\_token  | 访问令牌   | String | fdae8922a3b3d06a4e40882ac9f37a7e |
| expire\_times  | 会话过期时间 | String | 1389262844（10位）                  |
| refresh\_token | 刷新令牌   | String | 0d5ddfa364d51359e6243892bf0a965c |

###4.2、判断用户是否登录接口

```
isLogin(CallbackListener listener);
```
>调用该方法，会返回用户是否登录。

**参数**：  
CallbackListener监听器类  

**返回结果**：   
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为：  

| key     | 含义   | 类型      | 样例    |
| ------- | ---- | ------- | ----- |
| logined | 是否登录 | Boolean | false |

(2)失败时执行onFailed()方法，返回BSGameSdkError类型变量。  
(3)错误时执行onError()方法，返回BSGameSdkError类型变量。 

### 4.3、通知用户区服角色信息接口

```
notifyZone(String server_id, String server_name, String role_id, String role_name);
```

> 调用该方法来设置用户当前信息，用于支付校验。

> **注意：**

> **请在用户登录并选择角色以及服务器后调用，否则无法通过审核** 

> **notifyZone一次登录只能调用一次，不能多次调用。**

### 4.4、用户创建角色接口

```
gameSdk.createRole(role, role_id);
```

> 这个方法需要在用户创建角色成功时调用。  

**参数**：  

| 参数名称     | 参数说明              |
| -------- | ----------------- |
| role     | 用户在游戏内角色名(游戏自己的)  |
| role\_id | 用户在游戏内角色ID(游戏自己的) |

### 4.5、支付接口

> 支付之前请确认调用过notifyZone接口来设置当前区服信息,无需每次支付前都调用。**PS：该支付结果仅作为参考，真实结果请以服务器结果为准**

支付流程图： 

![支付流程](../img/支付接口逻辑图.jpg)

```
pay(int uid, String username, String role, String serverId, int total_fee, int game_money, String out_trade_no, String subject, String body, String extension_info, CallbackListener listener)
```

> 调用该方法，会打开平台支付页面，引导用户完成支付交易过程。 

**参数**：  

| 参数名称            | 参数说明                                     |
| --------------- | ---------------------------------------- |
| uid             | bilibili平台用户的唯一标识                        |
| username        | bilibili平台用户昵称                           |
| role            | 用户游戏内角色名                                 |
| serverId        | bilibili分配的区服id                          |
| total\_fee      | 本次交易金额，单位：分（注意，total_fee的值必须为整数，并且在1~100000之间) |
| game\_money     | 游戏内货币，即本次交易购买的游戏内货币                      |
| out\_trade\_no  | 商户订单号，8-32位字符，用于对账用                      |
| subject         | 商品名称，如：金币。（由于支付宝不支持特殊字符 % &，所以参数中不能包含 % &） |
| body            | 商品简单描述。（参数中不能包含 % &）                     |
| extension\_info | 支付接口的额外参数，会在服务器异步回调中原样传回                 |
| listener        | CallbackListener：监听器类                    |

**返回结果**：   
(1)成功时执行onSuccess()方法，返回String类型变量out_trade_no, bs_trade_no（我方的订单号）。  
(2)失败时执行onFailed()方法，返回String类型变量out_trade_no以及BSGameSdkError类型变量。

> 错误码：7005。返回此异常的场景为，当CP进行支付时传入的uid与SDK本地存储的uid不同时SDK会返回7005的error code。  
> 错误码：7004。返回此异常的场景为，新版微信支付过程中SDK通知发货失败，此时会返回7004的error bilibili sever查单接口），也可以等待bilibili server异步通知。  
> 错误码：-5。订单签名异常。

(3)错误时执行onError()方法，返回String类型变量out_trade_no以及BSGameSdkError类型变量。

### 4.6、账号失效监听接口

```
gameSdk.setAccountListener(new AccountCallBackListener() {
			
	@Override
	public void onAccountInvalid() {
		//todo 其他登出操作
		makeToast("用户已登出");
	}
});
```

> 此接口会在用户登录失效时调用，请在收到监听时进行相关登出操作，回到游戏登录界面

###4.7、获取用户信息接口

```
getUserInfo(CallbackListener listener);
```
>调用该方法，如果用户已经登录且没有超时，则返回用户相关信息。

**参数**：  
CallbackListener监听器类

**返回结果**：   
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为：  

| key               | 含义     | 类型     | 样例                               |
| ----------------- | ------ | ------ | -------------------------------- |
| uid               | 用户id   | String | 10001                            |
| username          | 用户昵称   | String | gamenick                         |
| access\_token     | 访问令牌   | String | fdae8922a3b3d06a4e40882ac9f37a7e |
| refresh\_token    | 刷新令牌   | String | 0d5ddfa364d51359e6243892bf0a965c |
| expire\_times     | 会话过期时间 | String | 1389262844（10位）                  |
| last\_login\_time | 最后登录时间 | String | 1389262844936                    |

(2)失败时执行onFailed()方法，返回BSGameSdkError类型变量。  
(3)错误时执行onError()方法，返回BSGameSdkError类型变量。

###4.8、登出接口

```
logout(CallbackListener listener);
```
>调用该方法，会返回用户是否登出成功。

**参数**：  
CallbackListener监听器类

**返回结果**：  
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为：   

| key  | 含义   | 类型     | 样例   |
| ---- | ---- | ------ | ---- |
| tips | 提示   | String | 注销成功 |

(2)失败时执行onFailed()方法，返回BSGameSdkError类型变量。  
(3)错误时执行onError()方法，返回BSGameSdkError类型变量。

### 4.9、判断当前用户是否实名认证接口

```
isRealNameAuth(CallbackListener listener);
```
>调用该方法，会返回用户是否实名认证。

**参数**: 
CallbackListener监听器类  

**返回结果**:  
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为： 

| key            | 含义         | 类型      | 样例    |
| -------------- | ---------- | ------- | ----- |
| isRealNameAuth | 当前用户是否实名认证 | Boolean | false |

(2)失败时执行onFailed()方法，返回BSGameSdkError类型变量。  
(3)错误时执行onError()方法，返回BSGameSdkError类型变量。 

##5、SDK返回值说明
### 5.1、返回值列表

| 参数名称              | 参数说明    | 类型     | 样例                               |
| ----------------- | ------- | ------ | -------------------------------- |
| result            | 结果状态    | String | -1                               |
| uid               | 用户id    | String | 10001                            |
| username          | 用户昵称    | String | gamenick                         |
| access\_token     | 访问令牌    | String | fdae8922a3b3d06a4e40882ac9f37a7e |
| refresh\_token    | 刷新令牌    | String | 0d5ddfa364d51359e6243892bf0a965c |
| last\_login\_time | 最后登录时间  | String | 1389262844936                    |
| expire\_times     | 会话过期时间  | String | 1389262844（10位）                  |
| bs\_trade\_no     | 我方订单号   | String | 20140101012345678                |
| out\_trade\_no    | CP商户订单号 | String | 20140101012345678                |
| logined           | 登录状态    | String | true                             |

>注：所有返回值均为json格式，格式化成字符串返回。

### 5.2、客户端状态代码

| 状态代码（result） | 状态描述 |
| ------------ | ---- |
| 1            | 操作成功 |
| -1           | 操作失败 |

### 5.3、客户端错误代码

| 错误代码(error.code) | 错误描述(error.msg) |
| ---------------- | --------------- |
| 100X             | 数据格式验证错误        |
| 200X             | 服务器返回异常         |
| 300X             | 未登录或者会话已超时      |
| 4000             | 系统错误            |
| 6001             | 用户中途取消          |


| 错误代码(error.code) | 错误描述(error.msg)                   |
| ---------------- | --------------------------------- |
| 1000             | 支付失败                              |
| 2001             | 服务器返回数据异常/网络未连接                   |
| 2002             | 网络未连接                             |
| 3001             | 用户未登录或登录已超时                       |
| 3002             | 用户未登录或登录已超时                       |
| 3003             | 注销失败                              |
| 6001             | 用户取消注册                            |
| 6002             | 用户取消登录                            |
| 7004             | 查单超时                              |
| 7005             | uid不统一支付失败                        |
| 8001             | 支付前请先调用nofiyZone方法通知区服，并确保与支付参数相符 |
| 91001            | 关闭登录                              |
| -1               | AppKey不存在或者已封禁                    |
| -2               | 无效的登录Token(登录已过期)                 |
| -3               | 无效的API签名(程序错误)                    |
| -14              | 游戏预下线，已关闭充值                       |
| -15              | 游客充值关闭                            |
| -101             | 未登录                               |
| -102             | 帐号已封禁                             |
| -103             | 积分不足                              |
| -104             | 硬币不足                              |
| -105             | 与验证码图片不匹配                         |
| -201             | 抽奖还未开始                            |
| -202             | 抽奖已结束                             |
| -203             | 网站功能                              |
| -400             | 请求错误(参数不合法,请求方式不正确)               |
| -403             | 拒绝访问(未登录,或用户权限不足)                 |
| -404             | 请求的内容不存在                          |
| -444             | 服务端维护中                            |
| -500             | 服务器内部错误                           |
| -501             | 服务器系统错误                           |
| -502             | 服务器API错误                          |
| -503             | 服务器调用太快                           |
| -621             | 邮箱格式不合法                           |
| -624             | 激活次数超过限制                          |
| -628             | 被泄露过的密码                           |
| -662             | 登录的RSA过期                          |
| -707             | 注册过频繁                             |
| -900             | 游客注册关闭                            |
| -10001           | Json解析异常                          |
| 500001           | 游戏处于封测，账号未激活                      |
| 500002           | 密码错误                              |
| 500003           | 用户名不存在                            |
| 500004           | 密码错误次数过多                          |
| 500005           | 用户名过长                             |
| 500006           | 密码过短                              |
| 500007           | 用户名不合法                            |
| 500008           | 用户名已存在                            |
| 500009           | 邮箱已注册                             |
| 500010           | 无效的激活码                            |
| 500011           | 激活码已被使用                           |
| 500012           | 该游戏不需要激活                          |
| 500013           | 激活失败，激活码可能已被使用                    |
| 500014           | 无效的激活码(非此游戏)                      |
| 500015           | 电话号码不合法                           |
| 500016           | 电话已存在                             |
| 500017           | 验证号发送失败                           |
| 500018           | 添加订单失败                            |
| 500019           | 用户名不存在                            |
| 500020           | 昵称或者密码过短                          |
| 500021           | 昵称过长                              |
| 500022           | 昵称已存在                             |
| 500023           | 密码错误                              |
| 500024           | 密码不安全，请提高密码强度                     |
| 500025           | 今日验证码发送次数已达上限                     |
| 600000           | 服务器请求配置错误                         |
| 60005            | 熔断                                |
| 60006            | 限流                                |
| 900001           | 主站未知错误                            |
