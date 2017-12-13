## 最新版本

* v1.0.3(2017-10-30) 下载地址：[sdk和接入文档](https://pkg.biligame.com/tool/data-sdk/android/bilibili%E6%B8%B8%E6%88%8F%E6%95%B0%E6%8D%AESDK1.0.3.zip  "Title")(419K)  
  MD5: 6246256df80a0585f96033c94859e271  
  SHA1: f7bd53386d4888c36a4975aa176457fb4e9830f6   

## 接入视频指南

[点击下载哔哩哔哩游戏-数据SDK接入视频](https://pkg.biligame.com/tool/bili-open/video/SDK%E5%BC%95%E5%85%A5%E4%B8%8E%E5%88%9D%E5%A7%8B%E5%8C%96.mp4) 

## 文档目录结构说明
1、Demo APK
1.1、1.BSGameDataSdk_Android_Demo.apk
```
这个是Android的Demo项目的APK作为验证使用
```

2、Demo Project
2.1.bsgame_datasdk_android_demo
```
这个是Android的Demo项目，接入时可以根据接入文档参考这个项目进行接入
```
3、接入内容
>注意接入数据SDK拷贝库必须从这个文件夹拷贝，不要从Demo项目中拷贝

3.1、1.bsgamedatasdk_android_library_xxx.jar
```
这个jar包就是要接入的库，直接拷贝到项目中的对应位置即可
```

4、接入文档
> 接入的时候请参照接入文档里面的内容进行接入

4.1.BSGameDataSdk集成接口.docx
```
接入的时候请按照文档的说明进行接入，注意文档中特殊颜色标注的内容
```

## 修订记录
> 如果跨版本升级请按照下面逐步升级的方式来升级  

1、**版本：1.0.3**  

**主要修改**

```
修复AndroidManifest.xml中如果配置meta导致channel获取为0问题
```
**资源替换**
```
资源直接拷贝"接入内容"文件夹下的bsgamedatasdk_android_library_1.0.3.jar
```
* 检查必填参数是否已经填写  
* 检查AndroidManifest.xml文件中权限和Service是否已经添加  

2、**版本：1.0.2**

**主要修改**

```
完善切换功能
```
**资源替换**
```
资源直接拷贝"接入内容"文件夹下的bsgamedatasdk_android_library_1.0.2.jar
```

* 检查必填参数是否已经填写
* 检查AndroidManifest.xml文件中权限和Service是否已经添加


3、**版本：1.0.1**

**主要修改**

```
1.兼容游戏内切换账号，数据按照新账号进行统计
2.去掉AndroidManifest.xml文件中Application标签声明sdk_log_type
```
**资源替换**
```
资源直接拷贝"接入内容"文件夹下的bsgamedatasdk_android_library_1.0.1.jar
```
* 检查必填参数是否已经填写
* 检查AndroidManifest.xml文件中权限和Service是否已经添加

4、**版本：1.0.0**

**主要修改**

```
添加统计用户在线时长功能
```
**资源替换**
```
资源直接拷贝"接入内容"文件夹下的bsgamedatasdk_android_library_1.0.0.jar
```
* 检查必填参数是否已经填写
* 检查AndroidManifest.xml文件中权限和Service是否已经添加
