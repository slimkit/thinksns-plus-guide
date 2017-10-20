# Android 项目导入安装教程


### 一、项目导入
1. 将项目导入`AndroidStudio`，如果当前没有按装，请前往[官方](https://developer.android.google.cn/studio/index.html)下载安装并配置好工作环境，`Android` 项目的运行依赖于1`Java JDK`，如果电脑没有配置`JDK`,请前往[官方](http://www.oracle.com/technetwork/java/javase/downloads/index.html)下载并配置,`Android Studio 2.2`以后的版本是于`JDK`绑定的不需要单独下载。

![project-import1]

![project-import2]

> **注意：**
> 1. 第一次导入项目时间会比较久，会下载依赖包，请保持网络通畅.
> 2. 建议：在导入项目之前，可手动将项目的 `gradle` 版本修改为当前已有的版本，这样可以快速导入。（修改内容为：`gradel/gradle-wrapper.propertie` 和 `project` 的 `build.gradle`）

2. 当前 TS Plus 开发环境介绍

 - Android Studio 2.3.3
 - Gradle 版本 2.2.+，插件版本 3.3
 - 编译版本 25 ，Android 最低运行版本 Android 4.1(Api 16)
 - JDK 1.8
 - cpu 目前支持说明如下，会更具需求有所变动
 ```
    ndk {
             //选择要添加的对应cpu类型的.so库。
             abiFilters 'armeabi', 'armeabi-v7a', 'armeabi-v8a', 'x86', 'x86_64', 'mips', 'mips64'
         }
 ```







--------------------------------
[project-import1]:../image/project_import1.jpeg "项目导入1"
[project-import2]:../image/project_import2.jpeg "项目导入2"
[creat_signer1]:../image/creat_signer1.png "创建签名1"
[creat_signer2]:../image/creat_signer2.jpeg "创建签名2"
[creat_signer3]:../image/create_signer3.png "创建签名3"
[creat_signer4]:../image/create_signer4.png "命令创建签名"
[creat_signer5]:../image/create_signer5.png "命令创建签名"
