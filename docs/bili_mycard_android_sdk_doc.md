## MyCard-Android手游SDK集成指南

BSGameSdk海外版开发包（简称：SDK）主要用来向第三方应用程序提供便捷、安全以及可靠的登录、支付服务。本文主要描述SDK用户注册、登录、修改密码以及支付接口的使用方法，供合作伙伴的开发者接入使用。


## 1、SDK接入流程
### 1.1、接入前期准备 

接入前期准备工作包括商户签约和密钥配置，已完成商户可略过。
需要获取的参数包括：

参数名称 | 参数说明
---------|-----------
server_id   | 我方分配的服务器id，可能有多个
merchant_id | 我方分配的商户id
app_id      | 我方分配的游戏id
app_key     | 游戏客户端密钥
secret_key  | 游戏服务端密钥
facebook\_app_id | 我方分配的facebook的游戏id
google\_client_id | 我方分配的经google 认证客户端id
google\_api_key | 我方分配的google服务密钥
google\_app_id | 我方分配的google游戏id


这些参数请联系运营人员为您配置
* secret\_key请妥善保存，为了防止泄露，不能出现在手机客户端

### 1.2、SDK接入流程
#### 1.2.1、SDK导入
1、**Eclipse项目工程导入**  
将`BSGameSdk_android_foreign_library`库工程导入eclipse，并在目标工程properties->android->library添加此library project，请参考demo工程（注意，将`bsgamesdk_android_foreign_demo` 和`bsgamesdk_android_foreign_library` 两个工程同时导入)。
![eclipse导入图](../img/eclipse_import.png)  
        
2、**库拷贝导入**   
将接入内容目录下libs, res里的文件拷贝到目标工程的对应目录，并在目标工程里设置引用libs里的jar文件，即完成sdk 的导入。  

#### 1.2.2、assets配置
拷贝demo项目assets下distributor.txt，ext.txt以及service.html到游戏工程assets下。ext.txt的内容确保为空。distributor.txt的内容确保为100，如果生成的包不需要重签名则内容确保为1.（如不清楚请与我方确认）
>拷贝libs下的so文件时，请根据游戏具体编译情况导入。比如unity默认的FAT（ARMv7 + x86）,则复制armeabi-v7a和x86的so即可。Cocos2dx查看设置的APP_ABI如果默认不设置则拷贝armeabi或者查看编译出自己的so库有哪几种类型就拷贝对应类型的so。

#### 1.2.3、修改AndroidManifest.xml

(1)**包名规范**  
游戏客户端的 AndroidManifest.xml 中游戏包名请找运营人员配置。

>注意：在manifest标签中不要添加android:installLocation="preferExternal"，在华为等一些设备4.0以上的系统不支持设置首选项为SD卡安装应用，使用默认值就可以了。  

(2)**Application**

application必须设置为`tw.com.mycard.sdk.libs.PSDKApplication`或是他的子类

```
	<application android:label="@string/app_name"
		android:name="tw.com.mycard.sdk.libs.PSDKApplication"
		android:icon="@drawable/icon">

```

(3)**权限声明**

添加以下权限

```
    <uses-permission android:name="android.permission.CAMERA" />

    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />

    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.FLASHLIGHT" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    <uses-permission android:name="android.permission.READ_LOGS" />
```

(4)**在Application中添加activity标签**

```
<activity
            android:name="com.bsgamesdk.android.activity.ForeignPayActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgamesdk_customDialog"></activity>
        <activity
            android:name="com.bsgamesdk.android.activity.BSGameSdkAgreementActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@android:style/Theme.NoTitleBar.Fullscreen"></activity>
        <activity
            android:name="com.bsgamesdk.android.activity.Login_RegActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgamesdk_customDialog"></activity>
        <activity
            android:name="com.bsgamesdk.android.activity.LoadingActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgamesdk_customDialog"></activity>

        <activity
            android:name="com.bsgamesdk.android.activity.TouristActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgamesdk_customDialog"></activity>


        <activity
            android:name="com.bsgamesdk.android.activity.WelcomeActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgemsdk_activity_welcome_style"></activity>

        <activity
            android:name="com.bsgamesdk.android.activity.NoticeActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgamesdk_customDialog"></activity>

        <activity
            android:name="com.bsgamesdk.android.activity.PointActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgamesdk_customDialog"></activity>

        <activity
            android:name="com.bsgamesdk.android.activity.ExitActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgamesdk_customDialog"></activity>

        <activity
            android:name="com.bsgamesdk.android.activity.RegisterActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:launchMode="singleTop"
            android:theme="@style/bsgamesdk_customDialog"></activity>

        <activity
            android:name="com.bsgamesdk.android.activity.ResetPasswordActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:screenOrientation="sensorLandscape"
            android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
            android:launchMode="singleTop">
        </activity>
        <activity
            android:name="com.bsgamesdk.android.activity.LicenseAgreementActivity"
            android:configChanges="keyboardHidden|navigation|orientation|screenSize"
            android:theme="@style/bsgamesdk_customDialog"
            android:launchMode="singleTop"
            >
        </activity>

        <!-- faceook  start -->
        <activity
            android:name="com.facebook.FacebookActivity"
            android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:exported="true"
            android:label="fortestname"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />
        <!-- faceook  end -->

        <!-- google  start -->
        <meta-data
            android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />

        <activity
            android:name="com.google.android.gms.common.api.GoogleApiActivity"
            android:exported="false"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />


        <activity
            android:name="com.google.android.gms.auth.api.signin.internal.SignInHubActivity"
            android:excludeFromRecents="true"
            android:exported="false"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />

        <service
            android:name="com.google.android.gms.auth.api.signin.RevocationBoundService"
            android:exported="true"
            android:permission="com.google.android.gms.auth.api.signin.permission.REVOCATION_NOTIFICATION" />
        <!-- google  end -->


        <!-- mycard begin -->

        <activity
            android:name="tw.com.mycard.paymentsdk.PSDKActivity"
            android:screenOrientation="portrait" >
        </activity>


        <!-- zxing -->
        <activity
            android:name="com.google.zxing.CaptureActivity"
            android:screenOrientation="portrait"></activity>

        <activity
            android:name="soft_world.mycard.paymentapp.ui.SplashActivity"
            android:screenOrientation="portrait" />
        <activity
            android:name="soft_world.mycard.paymentapp.ui.MainActivity"
            android:screenOrientation="portrait"
            android:windowSoftInputMode="adjustPan" />
        <activity
            android:name="soft_world.mycard.paymentapp.ui.TrainActivity"
            android:screenOrientation="portrait" />
        <activity
            android:name="soft_world.mycard.paymentapp.ui.billing.BillingWebViewActivity"
            android:configChanges="orientation"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Dialog" />

        <!-- 異康 -->
        <activity
            android:name="soft_world.mycard.paymentapp.Ecom.ATMMenuActivity"
            android:screenOrientation="portrait" />
        <activity
            android:name="com.xmobilepay.xpaymentlibs.XCardTypeForm"
            android:screenOrientation="portrait" />
        <activity
            android:name="com.xmobilepay.xpaymentlibs.XPayCardPassWordForm"
            android:screenOrientation="portrait" />
        <activity
            android:name="com.xmobilepay.xpaymentlibs.XSmallPayCardPassWordForm"
            android:screenOrientation="portrait" />
        <activity
            android:name="com.xmobilepay.xpaymentlibs.PaymentResultForm"
            android:screenOrientation="portrait" />
        <activity
            android:name="com.xmobilepay.xpaymentlibs.PaymentErrResultForm"
            android:screenOrientation="portrait" />
        <!-- 遠傳電信 -->
        <activity
            android:name="com.fet.iap.activity.FetLoginActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:theme="@android:style/Theme.Translucent.NoTitleBar"
            android:windowSoftInputMode="adjustPan" />

        <!-- 中華電信 -->
        <activity android:name="com.cht.iap.api.ChtRegMainActivity" />
        <activity android:name="com.cht.iap.api.ChtPhoneNumPayConfirmActivity" />
        <activity android:name="com.cht.iap.api.ChtRegEInvoiceInfo" />
        <activity android:name="com.cht.iap.api.ChtRegVerifyOTP" />
        <activity android:name="com.cht.iap.api.ChtRegHNDataTabActivity" />
        <activity android:name="com.cht.iap.api.ChtRegHNAccountActivity" />
        <activity android:name="com.cht.iap.api.ChtRegMobileAuth" />
        <activity android:name="com.cht.iap.api.ChtRegMobileHNData" />
        <activity android:name="com.cht.iap.api.ChtTransactionAuth" />
        <activity android:name="com.cht.iap.api.ChtRegVerifyMessage" />
        <!-- 中國信託 -->
        <activity
            android:name="com.softmobile.ui.PayPageActivity"
            android:configChanges="orientation"
            android:screenOrientation="portrait" />

        <!-- 中華電信 InApp -->
        <activity
            android:name="com.cht.chtinappapi.chtiapapi"
            android:configChanges="orientation"
            android:screenOrientation="portrait" />
        <activity
            android:name="com.cht.chtinappapi.RegisterRequest"
            android:label="@string/app_name"
            android:theme="@android:style/Theme.Light.NoTitleBar">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />

                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.cht.chtinappapi.TranxRequest"
            android:label="@string/app_name"
            android:theme="@android:style/Theme.Light.NoTitleBar">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />

                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
            </intent-filter>
        </activity>

        <!-- 首信易 -->
        <activity
            android:name="com.payeasenet.token.lib.ui.TokenPayTypeCheckUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.CardTypeCheckUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.TokenCreateUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.TokenCreateResultUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.TokenPayUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEPayRelUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.TokenIntroductionUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.TokenUnBindedUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.MoreAboutUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEUpopInfoUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEUpopPayUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEIvrPayUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEQuickPayUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEUpmpPayUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEVisaPayUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEVisaInfoUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEVisaBillInfoUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEDebitBillInfoUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEQuickInfoUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />
        <activity
            android:name="com.payeasenet.token.lib.ui.PEUpmpInfoUI"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Light.NoTitleBar" />

        <!-- mycard end -->

```

(5)**修改res/values/strings.xml 文件**

必须将facebook和google相关参数配置到string.xml,才能正常使用第三方平台服务,请按照提示修改以下参数。

Facebook参数:

```
<string name="facebook_app_id">填入分配的facebook_app_id</string>

```

Google参数:

```
<string name="default_web_client_id" translatable="false">填入分配的google_client_id</string>
<string name="gcm_defaultSenderId" translatable="false">填入分配的google_client_id横线(-)前的数字部分(例如google_client_id为”123321-abc.com”,那么只需在此填入123321)</string>
<string name="google_api_key" translatable="false">填入分配的google_api_key</string>
<string name="google_app_id" translatable="false">填入分配的google_app_id</string>
<string name="google_crash_reporting_api_key" translatable="false">填入分配的google_api_key</string>

```

## 2、SDK接口说明
### 2.1、SDK初始化

```
BSGameSdk *gameSdk= BSGameSdk.initialize(Boolean debug, Activity c, String merchant_id, String app_id, String server_id, String app_key, Handler handler);
```

参数名称    | 参数说明
------------|--------------
debug       | 是否打开debug模式，正式包必须关闭，值为false
instance    | Activity实例
merchant_id | cpid商户id,由平台自动分配
app_id      | 每款应用在平台的唯一标识，由平台分配
server_id   | 我方分配的商户应用的服务器编号，一般用来区分区服，如果有多个区服，则填其中一个默认服务器，并在选择角色、区服后调用notifyZone接口传入正确的区服id
app_key     | 商户应用的客户端密钥，请勿使用服务器端密钥
handler     | looper为main looper的Handler对象

返回值：

初始化成功，返回BSGameSdk实例。

####  接口调用方式
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

### 2.2、SDK接口介绍
#### 2.2.1、登录接口
```
login(CallbackListener listener);  
```
>调用该方法，会打开BSGame用户登录界面，引导用户输入用户名、密码来完成登录过程。  

**参数**：  
CallbackListener监听器类  
**返回结果**：  
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为：  

key   | 含义 | 类型  | 样例
-------|-----|-------|-------
uid       | 用户ID  | String | 10001
username  | 用户名昵称| String | gamenick
access_token  | 访问令牌     | String   | fdae8922a3b3d06a4e40882ac9f37a7e
expire_times  | 会话过期时间	| String   | 1389262844（10位）
refresh_token | 刷新令牌     | String   | 0d5ddfa364d51359e6243892bf0a965c

#### 2.2.2、注册接口
```
register(CallbackListener listener);
```
>调用该方法，会打开用户注册界面，引导用户输入用户名、密码来完成注册过程。

**参数**：  
CallbackListener监听器类  
**返回结果**:   
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中不包含有意义字段。   
(2)失败时执行onFailed()方法，返回BSGameSdkError类型变量。  
(3)错误时执行onError()方法，返回BSGameSdkError类型变量。  

#### 2.2.3、判断用户是否登录接口
```
isLogin(CallbackListener listener);
```
>调用该方法，会返回用户是否登录。

**参数**：  
CallbackListener监听器类  
**返回结果**：   
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为：  

key   | 含义 | 类型  | 样例
-------|-----|-------|-------
logined | 是否登录 | Boolean | false

(2)失败时执行onFailed()方法，返回BSGameSdkError类型变量。  
(3)错误时执行onError()方法，返回BSGameSdkError类型变量。  

#### 2.2.4、获取用户信息接口
```
getUserInfo(CallbackListener listener);
```
>调用该方法，如果用户已经登录且没有超时，则返回用户相关信息。

**参数**：  
CallbackListener监听器类  
**返回结果**：   
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为：  

key | 含义 | 类型  | 样例
-------|-----|-------|-------
uid | 用户id | String | 10001
username | 用户昵称 | String | gamenick
access_token | 访问令牌 | String | fdae8922a3b3d06a4e40882ac9f37a7e
refresh_token | 刷新令牌 | String | 0d5ddfa364d51359e6243892bf0a965c
expire_times | 会话过期时间 | String | 1389262844（10位）
last_login_time | 最后登录时间 | String | 1389262844936

(2)失败时执行onFailed()方法，返回BSGameSdkError类型变量。  
(3)错误时执行onError()方法，返回BSGameSdkError类型变量。

#### 2.2.5、登出接口
```
logout(CallbackListener listener);
```
>调用该方法，会返回用户是否登出成功。

**参数**：  
CallbackListener监听器类  
**返回结果**：  
(1)成功时执行onSuccess()方法，返回Bundle类型变量，其中包含键和值为：   

key | 含义 | 类型  | 样例
----|-----|-------|-------
tips | 提示 | String | 注销成功

(2)失败时执行onFailed()方法，返回BSGameSdkError类型变量。  
(3)错误时执行onError()方法，返回BSGameSdkError类型变量。  
#### 2.2.6、通知用户区服角色信息接口
```
notifyZone(String server_id, String server_name, String role_id, String role_name);
```
调用该方法来设置用户当前信息，用于支付校验。  
>（1）**请在用户登录并选择角色以及服务器后调用，否则无法通过审核**。  
（2）**notifyZone一次登录只能调用一次，不能多次调用。**


**参数**

参数名称 | 参数说明
---|---
server_id | 我方分配的服务器id
server_name | 我方分配的服务器名称
role_id   | 游戏角色id
role_name | 游戏角色名称

#### 2.2.7、支付接口
>支付之前请确认调用过notifyZone接口来设置当前区服信息,无需每次支付前都调用。**PS：该支付结果仅作为参考，真实结果请以服务器结果为准**

支付流程图： 

![支付流程](../img/支付接口逻辑图.jpg)


```
pay(int uid, String username, String role, String serverId, int total_fee, int game_money, String out_trade_no, String subject, String body, String extension_info, String currency, CallbackListener listener)

```

>调用该方法，会打开平台支付页面，引导用户完成支付交易过程。 

**参数**：  

参数名称 | 参数说明
---------|---
uid | bilibili平台用户的唯一标识
username | bilibili平台用户昵称
role | 用户游戏内角色名
serverId | bilibili分配的区服id
total\_fee | 本次交易金额，单位：分（注意，total_fee的值必须为整数，并且在1~100000之间)
game\_money | 游戏内货币，即本次交易购买的游戏内货币
out\_trade_no | 商户订单号，8-32位字符，用于对账用
subject | 商品名称，如：金币。（由于支付宝不支持特殊字符 % &，所以参数中不能包含 % &）
body | 商品简单描述。（参数中不能包含 % &）
extension\_info | 支付接口的额外参数，会在服务器异步回调中原样传回
currency | 币种，只支持TWD 
listener | CallbackListener：监听器类

**返回结果**：   
(1)成功时执行onSuccess()方法，返回String类型变量out\_trade\_no, bs\_trade\_no（我方的订单号）。  
(2)失败时执行onFailed()方法，返回String类型变量out\_trade\_no以及BSGameSdkError类型变量。
>错误码：7005。返回此异常的场景为，当CP进行支付时传入的uid与SDK本地存储的uid不同时SDK会返回7005的error code。  
错误码：7004。返回此异常的场景为，新版微信支付过程中SDK通知发货失败，此时会返回7004的error bilibili sever查单接口），也可以等待bilibili server异步通知。  

(3)错误时执行onError()方法，返回String类型变量out\_trade\_no以及BSGameSdkError类型变量。

#### 2.2.8、账号失效监听接口(必接)  
```
gameSdk.setAccountListener(new AccountCallBackListener() {
			
	@Override
	public void onAccountInvalid() {
		//todo 其他登出操作
		makeToast("用户已登出");
	}
});
```
>此接口会在用户登录失效时调用，请在收到监听时进行相关登出操作，回到游戏登录界面

#### 2.2.9、用户创建角色接口（必接）

```
gameSdk.createRole(role, role_id);
```
>这个方法需要在用户创建角色成功时调用。  

**参数**：  

参数名称 | 参数说明 
--------|----------
role | 用户在游戏内角色名(游戏自己的)
role_id | 用户在游戏内角色ID(游戏自己的)


## 3、SDK返回值说明
### 3.1、返回值列表
参数名称|参数说明|类型|样例
-------|-------|----|---
result|结果状态|String|-1
uid|用户id|String|10001
username|用户昵称|String|gamenick
access_token|访问令牌|String|fdae8922a3b3d06a4e40882ac9f37a7e
refresh_token|刷新令牌|String|0d5ddfa364d51359e6243892bf0a965c
last\_login_time|最后登录时间|String|1389262844936
expire_times|会话过期时间|String|1389262844（10位）
bs\_trade_no|我方订单号|String|20140101012345678
out\_trade_no|CP商户订单号|String|20140101012345678
logined|登录状态|String|true

>注：所有返回值均为json格式，格式化成字符串返回。

### 3.2、客户端状态代码
状态代码（result） | 状态描述
----|---------
1 | 操作成功
-1 | 操作失败

### 3.3、客户端错误代码  

错误代码(error.code)  | 错误描述(error.msg)
------------|-------------------
100X | 数据格式验证错误
200X | 服务器返回异常
300X | 未登录或者会话已超时
4000 | 系统错误
6001 | 用户中途取消


错误代码(error.code)  | 错误描述(error.msg)
------------|-------------------
1000 | 支付失败
2001 | 服务器返回数据异常/网络未连接
2002 | 网络未连接
3001 | 用户未登录或登录已超时
3002 | 用户未登录或登录已超时
3003 | 注销失败
6001 | 用户取消注册
6002 | 用户取消登录
7004 | 查单超时
7005 | uid不统一支付失败
8001 | 支付前请先调用nofiyZone方法通知区服，并确保与支付参数相符
91001 | 关闭登录
-1 | AppKey不存在或者已封禁
-2 | 无效的登录Token(登录已过期)
-3 | 无效的API签名(程序错误)
-15 | 游客充值关闭
-101 | 未登录
-102 | 帐号已封禁
-103 | 积分不足
-104 | 硬币不足
-105 | 与验证码图片不匹配
-201 | 抽奖还未开始
-202 | 抽奖已结束
-203 | 网站功能
-400 | 请求错误(参数不合法,请求方式不正确)
-403 | 拒绝访问(未登录,或用户权限不足)
-404 | 请求的内容不存在
-444 | 服务端维护中
-500 | 服务器内部错误
-501 | 服务器系统错误
-502 | 服务器API错误
-503 | 服务器调用太快
-621 | 邮箱格式不合法
-624 | 激活次数超过限制
-628 | 被泄露过的密码
-662 | 登录的RSA过期
500001 | 游戏处于封测，账号未激活
500002 | 密码错误
500003 | 用户名不存在
500004 | 密码错误次数过多
500005 | 用户名过长
500006 | 密码过短
500007 | 用户名不合法
500008 | 用户名已存在
500009 | 邮箱已注册
500010 | 无效的激活码
500011 | 激活码已被使用
500012 | 该游戏不需要激活
500013 | 激活失败，激活码可能已被使用
500014 | 无效的激活码(非此游戏)
500015 | 电话号码不合法
500016 | 电话已存在
500017 | 验证号发送失败
500018 | 添加订单失败
500019 | 用户名不存在
500020 | 昵称或者密码过短
500021 | 昵称过长
500022 | 昵称已存在
500023 | 密码错误
600000 | 服务器请求配置错误
-10001 | Json解析异常







