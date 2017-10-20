# Android 签名文件创建


### 一、通过开发工具生成（推荐）
1. 将项目导入`AndroidStudio`，如果当前没有按装，请前往[官方](https://developer.android.google.cn/studio/index.html)下载安装并配置好工作环境，`Android` 项目的运行依赖于1`Java JDK`，如果电脑没有配置`JDK`,请前往[官方](http://www.oracle.com/technetwork/java/javase/downloads/index.html)下载并配置,`Android Studio 2.2`以后的版本是于`JDK`绑定的不需要单独下载。

![project-import1]

![project-import2]

> **注意：**
> 1. 第一次导入项目时间会比较久，会下载依赖包，请保持网络通畅.
> 2. 建议：在导入项目之前，可手动将项目的 `gradle` 版本修改为当前已有的版本，这样可以快速导入。（修改内容为：`gradel/gradle-wrapper.propertie` 和 `project` 的 `build.gradle`）

2. 打开`Android Studio` 点击顶部 `build —> Module(app) -> Generate Signed APK -> Create new` ，在`New Key Store`填上相关的信息点击`OK`就完成了

![creat_signer1]

![creat_signer2]

![creat_signer3]

### 二、通过命令行生成
下面以创建一个android_release.keystore为例，结尾有附图参考：您需要在电脑上下载`Android SDK`、`JDK`包，要使用到`JDK`中的`keytool`工具。

1. 在电脑上打开终端输入以下命令：
```
keytool -genkey –alias android_release -keyalg RSA -validity 20000 -keystore android_release.keystore
```
2. 更具键入命令的提示完成操作：
 * 输入keystore密码： 
 * 再次输入新密码: 
 * 您的名字与姓氏是什么？  
 * 您的组织单位名称是什么？ 
 * 您的组织名称是什么？  
 * 您所在的城市或区域名称是什么？ 
 * 您所在的州或省份名称是什么？ 
 * 该单位的两字母国家代码是什么？ CN CN=ZhouJiangHai, OU=jxust, O=jxust, L=ganzhou, ST=jiangxi, C=cn
 * 正确吗？   y 
 * 输入<android_release.keystore>的主密码      （如果和 keystore 密码相同，按回车）： 这时会生成android_release.keystore文件，就是我们需要的签名文件;
（-validity 20000 表示证书的有效天数为20000天）

![creat_signer4]


### 三、查看 `MD5` 和 `SH1`

1. 在电脑上打开终端输入以下命令：
```
keytool –list –v –keystore C:/thinksns-plus-android/android_release

```

> ***注意：*** `C:/thinksns-plus-android/android_release `为当前文件的绝对路径

![creat_signer5]














--------------------------------
[project-import1]:../image/project_import1.jpeg "项目导入1"
[project-import2]:../image/project_import2.jpeg "项目导入2"
[creat_signer1]:../image/creat_signer1.png "创建签名1"
[creat_signer2]:../image/creat_signer2.jpeg "创建签名2"
[creat_signer3]:../image/create_signer3.png "创建签名3"
[creat_signer4]:../image/create_signer4.png "命令创建签名"
[creat_signer5]:../image/create_signer5.png "命令创建签名"
