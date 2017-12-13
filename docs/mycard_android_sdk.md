## 最新版本

* v1.0.2(2017-10-25) 下载地址：[sdk和接入文档](http://xxx  "Title")(??.?M)  
  MD5: ??  
  SHA1: ???  


## 文档目录结构说明  

1、Demo APK  
1.1、BSGameSdk_ Android_Demo.apk  
```
是安卓项目的Demo
```  
1.2、BSGameSdk_ Cocos2dx_Demo.apk  
```
是Cocos2dx项目的Demo，如果接入有问题或者看文档不清晰，可以查看BSGameSdk_Cocos2dx_Demo.apk的效果进行对比
```
1.3、BSGameSdk_ Unity3D_Demo.apk  
```
是Unity3D项目的Demo，如果接入有问题或者看文档不清晰，可以查看BSGameSdk_Unity3D_Demo.apk的效果进行对比
```
2、Demo Project  
>  Demo Project文件夹中包括安卓平台，Cocos2dx平台，Unity3D平台以及后台项目的Demo，可以对比接入文档查看接入的正确性。；   

**运行方法**：  
>  将Demo项目（bsgamesdk_androidstudio_demo\bsgamesdk_android_demo）通过Open或者Import Project添加到AndroidStudio工程中；

2.2、Android Demo Project  
>	Android Demo 项目包括Demo项目（bsgamesdk_android_demo）和依赖库项目（bsgamesdk_android_library）； 

**运行方法**：  
>  (1)将Demo项目（bsgamesdk_android_demo）和依赖库项目（bsgamesdk_android_library）两个工程同时导入到eclipse中；  
>  (2)设置Demo项目的properties->android->library添加此依赖库项目,然后运行Demo项目就可以查看效果。  

2.3、BSGameSdk_ Cocos2dx_Demo
```
这个是Cocos2dx的Demo项目
```
2.4、BSGAMESDK_ Unity3D_DEMO
```
这是Unity3D的Demo项目
```
2.5、sdk-server-demo
```
这是服务器端的Demo项目
```

3、接入内容
> 这个文件内容为研发接入SDK需要拷贝的资源，包括调用java的代码、SO库、res资源文件以及jar文件   

具体内容查看里面的“接入内容说明.md”文件。


4、接入文档  
4.1、BSGameSdk_Android_Bili接入文档.docx
```
SDK接入请参考这个文档，里面标注注意点以及必须接入的接口。
```
4.2、BSGameSdk客户端接入FAQ.docx
```
接入过程中遇到的问题的收集，如果出现问题可以查看这个文档是否已经解释，如果没有可以通过讨论组进行询问。
```
4.3、BSGame游戏SDK服务端API接口使用手册.doc
```
服务器端接入请参考这个文档，其中可能返回值类型与给出的字段类型不一致，以实际返回值为准
```
4.4、BSGame游戏SDK开发参考说明书-总体机制(Andoird版).doc
```
主要是讲解SDK的流程包括总体架构，登录机制，充值机制和接入步骤。
```
4.5、BSGameSdk海外版客户端接入  FAQ
```
服务器端接入请参考这个文档，其中可能返回值类型与给出的字段类型不一致，以实际返回值为准
```

## 修订记录

### 升级说明
> 如果跨版本升级请按照下面逐步升级的方式来升级


### 1.0.1 - 1.0.2
>关闭用户名密码注册页面

#### 资源替换

`libs`下的`bsgamesdk_android_foreign_mycard-1.0.1.jar` 替换成 `bsgamesdk_android_foreign_mycard-1.0.2.jar`;



### 1.0.0 - 1.0.1
>增加找回密码H5版,添加协议授权提示


#### 主要修改
```
1.增加h5版本"找回密码"功能
2.增加游客登录、三方登录(FaceBook和Google)前协议授权提示
```

#### 资源替换
```
1.删除之前版本的所有资源文件(res目录下属于海外SDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_foreign_library-1.0.0.jar 替换成 bsgamesdk_android_foreign_library-1.0.1.jar;
3.AndroidManifest文件,请按接入文档改动。
```



### 1.0.0


#### 支付接口变化

新增`currency`参数，只支持传`TWD`，代表台币

```
/**
     * @param uid            BSGame平台用户的唯一标识(整型)
     * @param username       用户名或者email（唯一）
     * @param role           充值的角色信息
     * @param serverId       区服号
     * @param total_fee      充值金额
     * @param game_money     游戏币，需要用充值金额*充值比率
     * @param out_trade_no   充值订单号
     * @param subject        充值主题
     * @param body           充值描述
     * @param extension_info 附加信息，会在服务器异步回调中原样传回
     * @param currency       币种，目前只支持TWD代表台币
     * @param listener       用于执行结束后回调的监听器类
     */
    public void pay(final String uid, final String username, final String role, final String serverId, final int total_fee,
                    final int game_money, final String out_trade_no, final String subject, final String body,
                    final String extension_info, final String currency, final OrderCallbackListener listener)
```

#### manifest变动

##### Application
application必须设置为```tw.com.mycard.sdk.libs.PSDKApplication```或是他的子类

```
 <application  
 	android:label="@string/app_name"
	android:name="tw.com.mycard.sdk.libs.PSDKApplication"
	android:icon="@drawable/icon">
```

##### uses-permission
请删除原来所需添加的权限并添加以下`uses-permission`

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

##### activity

请参考文档以及demo添加mycard需要的相关`activity`

##### 资源变化

删除之前版本的所有资源文件(res目录下属于海外SDK的),重新复制"接入内容"下的res文件夹;