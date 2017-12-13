<h1>哔哩哔哩游戏数据SDK集成指南</h1>
哔哩哔哩游戏数据SDK开发包（简称：DataSDK）主要用来向第三方应用程序提供便捷实时数据统计。本文主要描述DataSDK的使用方法，供合作伙伴的开发者接入使用。  

## 1、接入流程

### 1.1、接入前准备

接入前期准备工作包括商户签约和密钥配置，已完成商户可略过。
需要获取的参数包括：

| 参数名称        | 参数说明             |
| ----------- | ---------------- |
| server_id   | 我方分配的服务器id，可能有多个 |
| merchant_id | 我方分配的商户id        |
| app_id      | 我方分配的游戏id        |
| app_key     | 游戏客户端密钥          |
| secret_key  | 游戏服务端密钥          |

**这些参数请联系运营人员为您配置**

* app_key是客户端签名所使用的key,在sdk初始化的时候必须参数     
* secret_key请妥善保存，**为了防止泄露，不能出现在手机客户端** 

### 1.2、SDK接入流程

#### 1.2.1、导入SDK

将bsgamedatasdk_android_library_xxx.jar拷贝到目标工程的对应目录，并在目标工程里设置引用libs里的jar 文件，即完成sdk 的导入。  
#### 1.2.2、修改AndroidManifest.xml 文件
在<application>内添加
```
<!-- data-collect start-->
        <service
            android:name="com.bsgamesdk.android.dc.service.DcUpService"
            android:enabled="true"
            android:exported="true">
        </service>
 <!-- data-collect end-->
```
#### 1.2.3、assets配置
如果接入哔哩哔哩游戏SDK查看assets下有没有distributor.txt文件，如果有则不需要修改，如果没有则拷贝demo下的distributor.txt文件，内容为100（如果是我方签名则内容为100，否则内容为1）   

## 2、SDK接口说明
### 2.1、SDK初始化
```
DataCollect.getInstance().dCInit(Activity activity, DataParamsModel dataParamsModel);
```
* activity: Activity实例；
* dataParamsModel:是需要传入参数的对象，类的结构如下：  

| 字段名称        | 类型     | 标注               |
| ----------- | ------ | ---------------- |
| merchant_id | String | cpid商户id,由平台自动分配 |
| server_id   | String | 平台分配的区服id        |
| app_id      | String | 每款应用在平台的唯一标识     |
| uid         | String | bilibili用户id     |

>**注意：DataParamsModel 的参数必须全部填写，如果没有填写DataSDK会进行校验提示具体哪个参数没有填写,由于uid需要登录之后才能获取，所以dCInit需要登录之后进行初始化,不要多次调用。**

### 2.2、接口介绍
#### 2.2.1、程序回到前台接口appOnline（必接）
```
@Override
protected void onResume() {
    super.onResume();
    DataCollect.getInstance().appOnline();
}
```
#### 2.2.2、程序进入后台接口appOffline（必接）
```
@Override
protected void onStop() {
    super.onStop();
    DataCollect.getInstance().appOffline();
}
```
#### 2.2.3、程序退出接口appDestory（必接）
```
@Override
protectesd void onDestroy() {
    super.onDestroy();
    DataCollect.getInstance().appDestory();
}
```
