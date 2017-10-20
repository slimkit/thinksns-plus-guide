# Android 打包教程

### 一、打测试版
1. 导入项目，具体导入步骤可以查看 [创建Android签名文件](AndroidCreateSignatureFileTutorial.md)
2.
 * 直接 `build` ,点击 `Build -> Build APK` 等待完成即可。

![build_apk1]

 * 通过运行到设备构建，点击 `运行按钮`，选择需要运行的设备，点击 `Ok` 即可。

![build_apk2]

> **注意：** 生成的文件地址：`thinksns-plus-android/app/build/outputs/apk/app-debug.apk`


### 二、打正式版本

1. 导入项目。
2. 点击 `Build -> Generate Signed APK`，如果已经创建了签名，直接选择签名，填上秘钥 `Next -> Finish` 等待完成即可；如果还没有签名，请先 [创建签名](AndroidPackageTutorial.md)

![build_apk3]

![build_apk4]

![build_apk5]


### 三、让 `Debug 版本` 使用正式版的签名
打开项目目录下的 `app Module` 地址：`thinksns-plus-android/app/build.gradle`
将 `SigningConfigs` 的 `debug` 版的签名证书为发布版的签名证书。

![build_apk6]











--------------------------------
[build_apk1]:../image/build_apk1.jpeg "构建项目"
[build_apk2]:../image/build_apk2.jpeg "构建项目"
[build_apk3]:../image/build_apk3.jpeg "构建项目"
[build_apk4]:../image/build_apk4.jpeg "构建项目"
[build_apk5]:../image/build_apk5.jpeg "构建项目"
[build_apk6]:../image/build_apk6.jpeg "构建项目"