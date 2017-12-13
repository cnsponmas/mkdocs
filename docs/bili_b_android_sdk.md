## 最新版本

* v1.6.7(2017-10-30) 下载地址：[sdk和接入文档](https://pkg.biligame.com/tool/bili-open/b_bili/%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9%E6%B8%B8%E6%88%8FSDK1.6.7%EF%BC%8820171030-1%EF%BC%89.zip  "Title")(279.8M)  
  MD5: ebcefaabbcd4a70801455942bd3e965e  
  SHA1: 8c5a2d177ad2946fb387f325f934376b0022d36f  

## 接入视频指南

[SDK引入和初始化](https://pkg.biligame.com/tool/bili-open/video/SDK%E5%BC%95%E5%85%A5%E4%B8%8E%E5%88%9D%E5%A7%8B%E5%8C%96.mp4)

[支付](https://pkg.biligame.com/tool/bili-open/video/%E6%94%AF%E4%BB%98.mp4)

[登录和创角](https://pkg.biligame.com/tool/bili-open/video/%E7%99%BB%E5%BD%95%E4%B8%8E%E5%88%9B%E8%A7%92.mp4)

[目录结构](https://pkg.biligame.com/tool/bili-open/video/%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.mp4)

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
```
Demo Project文件夹中包括安卓平台，Cocos2dx平台，Unity3D平台以及后台项目的Demo，可以对比接入文档查看接入的正确性。
```

2.1、AndroidStudio Demo Project  
```
AndroidStudio Demo 项目包括Demo项目（bsgamesdk_androidstudio_demo\bsgamesdk_android_demo）,通过依赖aar包的形式依赖库项目；   
```

**运行方法**：  
```
将Demo项目（bsgamesdk_androidstudio_demo\bsgamesdk_android_demo）通过Open或者Import Project添加到AndroidStudio工程中；
```

2.2、Android Demo Project  
```
Android Demo 项目包括Demo项目（bsgamesdk_android_demo）和依赖库项目（bsgamesdk_android_library）； 
```

**运行方法**： 

* 将Demo项目（bsgamesdk_android_demo）和依赖库项目（bsgamesdk_android_library）两个工程同时导入到eclipse中；  
* 设置Demo项目的properties->android->library添加此依赖库项目,然后运行Demo项目就可以查看效果。  

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
4.5、BSGame游戏加速层服务端API接口使用手册_阿里云.doc
```
服务器端接入请参考这个文档，其中可能返回值类型与给出的字段类型不一致，以实际返回值为准
```
4.6、BSGame游戏加速层服务端API接口使用手册_腾讯云.doc
```
服务器端接入请参考这个文档，其中可能返回值类型与给出的字段类型不一致，以实际返回值为准
```
5、自检工具
> 这个文件内容为研发接入SDK时,使用检查工具确认是否正确接入SDK,根据检查结果重新接入SDK   

具体内容查看里面的“使用说明.md”文件。

## 修订记录

> 如果跨版本升级请按照下面逐步升级的方式来升级

1、**1.6.5 - 1.6.6**
>登录、支付等部分内容优化

主要修改
```
1.登录线路优化
2.支付优化
3.PaymentActivity改为竖屏，此部分请参考接入文档
4.为Android Studio接入方式增加混淆文件，具体请参看接入内容
```
2、**1.6.4 - 1.6.5**
>提供判断用户是否实名认证接口
>优化Paypal支付
>更新SDK客户端错误码文档

主要修改
```
1.提供判断用户是否实名认证接口
2.优化Paypal支付
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.6.4.jar 替换成 bsgamesdk_android_library_1.6.5.jar;
```

3、1.6.3.1 - 1.6.4
>优化登录相关线路切换  
>增加GUI版自检工具  
>更新并增加服务端文档  


主要修改
```
1.优化登录相关线路切换
2.增加限流功能,优化网络访问
3.修复创角接口bug
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.6.3.1.jar 替换成 bsgamesdk_android_library_1.6.4.jar;
```

4、**1.6.3 - 1.6.3.1**
>修复已知bug  
>增加自检工具  

主要修改
```
1.修复已知bug
2.增加自检工具
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.6.3.jar 替换成 bsgamesdk_android_library_1.6.3.1.jar;
3.libs下的.so文件,请按文档拷贝;
4.AndroidManiest文件,请按文档改动。
```

5、**1.6.2.3 - 1.6.3**
>新版微信支付

主要修改
```
1.新版微信支付功能
2.修复已知bug
注：支付增加了错误码，详情参考接入文档
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.6.2.3.jar 替换成 bsgamesdk_android_library_1.6.3.jar;
3.libs下的.so文件,请按文档拷贝;
4.AndroidManiest文件,请按文档改动。
```

6、**1.6.2.2 - 1.6.2.3**
>兼容添加了微信分享的游戏

主要修改
```
1.将微信功能jar包更换为完整jar包,可以支持微信分享和微信支付
2.修复实名认证在开启封测时的UI异常
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.6.2.2.jar 替换成 bsgamesdk_android_library_1.6.2.3.jar;
3.libs下的.so文件,请按文档拷贝;
4.AndroidManiest文件,请按文档改动。
```

7、**1.6.2.1 - 1.6.2.2**
>bug修复

主要修改
```
1.实名认证界面在横屏显示不全的情况下不可滑动的bug
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.6.2.1.jar 替换成 bsgamesdk_android_library_1.6.2.2.jar;
3.libs下的.so文件,请按文档拷贝;
4.AndroidManiest文件,请按文档改动。
```

8、**1.6.2 - 1.6.2.1**
>bug修复

主要修改
```
1.修复游客登录在某种情况可绕过实名认证的bug
2.okHttp三方框架保持不混淆
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.6.2.jar 替换成 bsgamesdk_android_library_1.6.2.1jar;
3.libs下的.so文件,请按文档拷贝;
4.AndroidManiest文件,请按文档改动。
```

9、**1.5.8 - 1.6.2**
>登录实名认证  
>paypal支付升级  
>网络切线优化  

主要修改
```
1.登录(用户登录、一键登录、游客登录、自动登录)前实名认证
2.paypal支付升级(6月30日之前仍然使用以前WebView的支付方式;6月30日及以后将使用本地SDK)
3.网络切线优化
```

资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.5.8.jar 替换成 bsgamesdk_android_library_1.6.2.jar;
3.libs下的.so文件已增加,请按文档对应拷贝;
4.AndroidManiest有变动,请根据demo拷贝对应改动。
```
10、**1.5.7 - 1.5.8**
>增加微信支付功能

主要修改
```
1.增加微信支付功能
2.修复若干特殊机型切换账号卡死问题
3.更换logo图片
4.修复账号安全性bug
```

资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.5.7.jar 替换成 bsgamesdk_android_library_1.5.8.jar。
```

11、**1.5.5 - 1.5.7**
>修复兼容性问题,增加收集异常日志功能

主要修改
```
1.添加主动收集异常日志功能
2.修复6.0及其以上系统无法正确获取wifi mac值问题
3.修复平板杀死进程无法自动登录问题
4.修复支付宝白屏问题
5.优化网路请求切线效率，缩短超时时间
6.增加arm64-v8a、mips64、x86_64架构so文件
7.修复平板中界面长宽比过大问题
```

资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.5.5.jar 替换成 bsgamesdk_android_library_1.5.7.jar。
```

12、**1.5.4 - 1.5.5**
>增加实名认证功能

主要修改
```
1.普通注册、手机注册、游客升级成功后需实名认证
2.增加普通注册成功后提示界面
3.优化登出、获取用户信息、判断是否登录接口
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.5.4.jar 替换成 bsgamesdk_android_library_1.5.5.jar。
```


13、**1.5.3 - 1.5.4**
>此版本主要解决之前版本发现的异常问题

主要修改
```
1.解决在游客或者用户自动登录时，用户刷机导致设备ID更改,读取的密钥发生了变化导致解密失败出现异常；
2.解决手机内存不够,对话框依附的界面销毁重建,调用关闭对话框出现的异常；
3.解决异步任务返回之前界面消失，从而引起空指针异常；
4.解决并发同时操作切线列表导致异常；
5.解决用户自动登录Token过期并且需要激活进入游戏时出现的异常；
6.解决公告界面网络请求过程中发生中断导致的空指针异常。
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.5.3.jar 替换成 bsgamesdk_android_library_1.5.4.jar。
```


14、**1.5.2 - 1.5.3**
> 升级到此版本注意资源替换中res文件问题；
> 具体接入请按照接入文档来，最新的接入文档内容针对这版也进行了修改。

主要修改
```
1.修复关于SDK在某些机型的兼容性问题；
2.修改密码修改中，新密码与原密码相同的提示信息；
3.修改混淆方式，保持包名为com.bsgamesdk；
4.添加当封档测试时自动登录进行校验是否激活。
```
资源替换
```
1.删除之前版本的所有资源文件(res目录下属于BiliSDK的)，从新拷贝"接入内容"下的res文件夹；
2.libs下bsgamesdk_android_library_1.5.2.jar 替换成 bsgamesdk_android_library_1.5.3.jar。
```

15、**1.5.1 - 1.5.2**
主要修改
```
关闭游客支付功能，
添加游客支付时的引导界面。
```
资源替换
```
libs下bsgamesdk_android_library_1.5.1.jar
替换成 bsgamesdk_android_library_1.5.2.jar

替换res目录下的所有资源文件
```
